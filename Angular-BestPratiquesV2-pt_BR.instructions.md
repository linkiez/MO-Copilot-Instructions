# Angular Best Practices & Guidelines

Este guia estabelece convenções de estilo para aplicações Angular, promovendo consistência no ecossistema Angular. Estas recomendações facilitam o compartilhamento de código e a transição entre projetos.

## Princípios Fundamentais

- **Consistência**: Priorize manter consistência dentro de um arquivo
- **Legibilidade**: Código deve ser claro e autodocumentado
- **Performance**: Otimize para desempenho sem sacrificar legibilidade
- **Manutenibilidade**: Estruture código para facilitar manutenção futura
- **Segurança**: Siga práticas de segurança Angular

## ⚠️ INSTRUÇÃO CRÍTICA - COMPATIBILIDADE

**🔴 MUITO IMPORTANTE: GARANTIR COMPATIBILIDADE DAS MUDANÇAS**

Antes de implementar qualquer alteração no código:

1. **Análise de Impacto**: Avalie como as mudanças afetam outros componentes, serviços e funcionalidades
2. **Backward Compatibility**: Mantenha compatibilidade com código existente sempre que possível
3. **Breaking Changes**: Se mudanças quebram compatibilidade, documente claramente e forneça migração
4. **Testes**: Execute testes existentes e crie novos para validar que nada foi quebrado
5. **Dependency Check**: Verifique se as mudanças afetam dependências internas ou externas
6. **API Consistency**: Mantenha consistência nas interfaces públicas de componentes e serviços
7. **Version Impact**: Considere o impacto em diferentes versões do Angular e bibliotecas
8. **🚨 CSS/SCSS Preservation**: NUNCA modifique estilos CSS/SCSS existentes sem expressa autorização. Apenas adicione novos estilos quando necessário, mantendo os existentes intactos para preservar o design e layout atual.
9. **🏗️ HTML Structure Preservation**: NUNCA altere a estrutura de tags HTML existentes (não adicione, remova ou mova elementos). APENAS permitido: atualizar referências para usar melhorias implementadas no component.ts (signals, computed, métodos, getters) e adicionar atributos de acessibilidade (ARIA, role, etc.) às tags existentes.

**Sempre pergunte antes de fazer mudanças que possam quebrar funcionalidades existentes!**

## Quick Reference - Checklist de Verificação

### ✅ Must-Have (Obrigatório)

- [ ] Usar `inject()` ao invés de injeção por construtor
- [ ] Aplicar `OnPush` change detection em componentes
- [ ] Usar `signal()` para estado reativo
- [ ] Marcar propriedades Angular como `readonly`
- [ ] Usar `protected` para membros usados em templates
- [ ] Implementar interfaces de lifecycle hooks
- [ ] Evitar lógica complexa em templates

### ✅ Performance Crítica

- [ ] Usar `trackBy` em \*ngFor
- [ ] Implementar lazy loading para módulos/rotas
- [ ] Usar `async` pipe para observables
- [ ] Evitar pipes impuros
- [ ] Usar `OnPush` change detection strategy
- [ ] Implementar virtual scrolling para listas grandes

### ✅ Segurança

- [ ] Sanitizar entradas do usuário
- [ ] Usar `bypassSecurityTrust*` apenas quando necessário
- [ ] Implementar CSP (Content Security Policy)
- [ ] Validar dados no frontend E backend

### ✅ CSS/SCSS

- [ ] 🚨 NUNCA modificar estilos CSS/SCSS existentes sem autorização explícita
- [ ] Preservar design e layout atual intactos
- [ ] Apenas adicionar novos estilos quando absolutamente necessário
- [ ] Usar CSS custom properties para consistência
- [ ] Evitar `!important` a menos que essencial
- [ ] Implementar responsive design com mobile-first

### ✅ HTML Template

- [ ] 🏗️ NUNCA alterar estrutura de tags HTML existentes (não adicionar/remover/mover elementos)
- [ ] ✅ PERMITIDO: Atualizar referências para usar melhorias do component.ts (signals, computed, métodos, getters, propriedades)
- [ ] ✅ PERMITIDO: Adicionar atributos de acessibilidade (ARIA, role, type, etc.)
- [ ] ✅ PERMITIDO: Trocar `{{ form.get('field')?.value }}` por `{{ fieldValue() }}` ou `{{ getFieldValue() }}`
- [ ] ✅ PERMITIDO: Usar novos métodos: `*ngIf="hasErrors()"` ao invés de `*ngIf="form.invalid"`
- [ ] ✅ PERMITIDO: Referenciar computed properties, getters, e métodos públicos/protected
- [ ] ✅ PERMITIDO: Adicionar `novalidate`, `aria-*`, `role`, `type` aos elementos existentes
- [ ] Preservar estrutura DOM exata e hierarquia de elementos

## 1. Naming & File Organization

### Arquivos e Estrutura

```typescript
// ✅ CORRETO - Separar palavras com hífens
user - profile.component.ts;
user - profile.component.html;
user - profile.component.scss;
user - profile.component.spec.ts;

// ❌ EVITAR
userProfile.component.ts;
UserProfile.component.ts;
```

### Estrutura de Projeto Recomendada

```
src/
├── app/
│   ├── core/           # Singleton services, guards, interceptors
│   ├── shared/         # Shared components, pipes, directives
│   ├── features/       # Feature modules
│   │   ├── user/
│   │   │   ├── components/
│   │   │   ├── services/
│   │   │   ├── models/
│   │   │   └── user.module.ts
│   └── layouts/        # Layout components
```

### Convenções de Nomenclatura

- **Componentes**: `UserProfileComponent` → `user-profile.component.ts`
- **Serviços**: `UserService` → `user.service.ts`
- **Pipes**: `CurrencyPipe` → `currency.pipe.ts`
- **Guards**: `AuthGuard` → `auth.guard.ts`
- **Interfaces**: `User` → `user.interface.ts`
- **Enums**: `UserRole` → `user-role.enum.ts`
- **Constantes**: `API_ENDPOINTS` → `api-endpoints.const.ts`

## 2. Dependency Injection & Modern Angular Patterns

### ✅ Usar inject() - Padrão Moderno

```typescript
// ✅ PREFERIDO - Modern inject function
@Component({
  selector: 'app-user',
  standalone: true,
})
export class UserComponent {
  private userService = inject(UserService);
  private router = inject(Router);
  private fb = inject(FormBuilder);

  // Melhor inferência de tipos
  private httpClient = inject(HttpClient);
}

// ❌ EVITAR - Constructor injection (legacy)
export class UserComponent {
  constructor(
    private userService: UserService,
    private router: Router,
    private fb: FormBuilder,
  ) {}
}
```

### Signals - Estado Reativo

```typescript
// ✅ USAR Signals para estado reativo
@Component({
  selector: 'app-counter',
  template: `
    <p>Count: {{ count() }}</p>
    <p>Double: {{ doubleCount() }}</p>
    <button (click)="increment()">+1</button>
  `,
})
export class CounterComponent {
  protected count = signal(0);
  protected doubleCount = computed(() => this.count() * 2);

  protected increment(): void {
    this.count.update((value) => value + 1);
  }
}
```

### Input/Output Moderno

```typescript
// ✅ MODERN - Signal-based inputs
@Component({
  selector: 'app-user-card',
  template: `<h2>{{ displayName() }}</h2>`,
})
export class UserCardComponent {
  readonly user = input.required<User>();
  readonly showEmail = input(false);
  readonly userClicked = output<User>();

  protected displayName = computed(() => (this.showEmail() ? `${this.user().name} (${this.user().email})` : this.user().name));
}
```

## 3. Components & Performance

### OnPush Change Detection Strategy

```typescript
// ✅ SEMPRE usar OnPush para melhor performance
@Component({
  selector: 'app-user-list',
  changeDetection: ChangeDetectionStrategy.OnPush,
  standalone: true,
  template: `
    <div *ngFor="let user of users(); trackBy: trackByUserId">
      {{ user.name }}
    </div>
  `,
})
export class UserListComponent {
  readonly users = input.required<User[]>();

  protected trackByUserId(index: number, user: User): number {
    return user.id; // ✅ SEMPRE usar trackBy com *ngFor
  }
}
```

### Component Best Practices

```typescript
@Component({
  selector: 'app-user-profile',
  changeDetection: ChangeDetectionStrategy.OnPush,
  standalone: true,
  template: `
    <h1>{{ fullName() }}</h1>
    <p>{{ formatBirthDate() }}</p>
    <button (click)="saveProfile()" [disabled]="isSaving()">
      {{ isSaving() ? 'Salvando...' : 'Salvar' }}
    </button>
  `,
})
export class UserProfileComponent implements OnInit, OnDestroy {
  // ✅ Propriedades Angular marcadas como readonly
  readonly firstName = input.required<string>();
  readonly lastName = input.required<string>();
  readonly birthDate = input<Date>();
  readonly profileSaved = output<UserProfile>();

  // ✅ Estado interno usando signals
  protected isSaving = signal(false);

  // ✅ Computed para lógica derivada
  protected fullName = computed(() => `${this.firstName()} ${this.lastName()}`);

  // ✅ Protected para membros usados no template
  protected formatBirthDate = computed(() => {
    const date = this.birthDate();
    return date ? date.toLocaleDateString('pt-BR') : 'Não informado';
  });

  private userService = inject(UserService);
  private destroyRef = inject(DestroyRef);

  ngOnInit(): void {
    this.loadUserPreferences();
  }

  // ✅ Métodos de evento nomeados pelo que fazem
  protected async saveProfile(): Promise<void> {
    this.isSaving.set(true);
    try {
      const profile = this.buildProfile();
      await this.userService.saveProfile(profile);
      this.profileSaved.emit(profile);
    } catch (error) {
      console.error('Erro ao salvar perfil:', error);
    } finally {
      this.isSaving.set(false);
    }
  }

  private loadUserPreferences(): void {
    // ✅ Lógica complexa em métodos separados
    this.userService
      .getUserPreferences()
      .pipe(takeUntilDestroyed(this.destroyRef))
      .subscribe((preferences) => {
        // Aplicar preferências
      });
  }

  private buildProfile(): UserProfile {
    return {
      firstName: this.firstName(),
      lastName: this.lastName(),
      birthDate: this.birthDate(),
    };
  }
}
```

### Template Best Practices

```html
<!-- ✅ CORRETO - Preservar estrutura, atualizar referências e acessibilidade -->
<div class="existing-class">
  <!-- ✅ Trocar referências diretas por melhorias do component.ts -->
  <p>{{ userDisplayName() }}</p> <!-- era: {{ form.get('name')?.value }} -->
  <span>{{ formatPhoneNumber() }}</span> <!-- era: {{ phone | phonePipe }} -->
  <div *ngIf="hasValidationErrors()">{{ getErrorMessage() }}</div> <!-- era: *ngIf="form.invalid" -->
  
  <!-- ✅ Usar computed properties, getters e métodos -->
  <img [src]="profileImageUrl()" /> <!-- computed property -->
  <button [disabled]="isSubmitting">{{ submitButtonText }}</button> <!-- getter -->
  
  <!-- ✅ Adicionar atributos de acessibilidade -->
  <input 
    type="email" 
    formControlName="email" 
    [attr.aria-describedby]="emailError() ? 'email-error' : null"
    [attr.aria-invalid]="hasEmailError()"
    role="textbox"
  />
  
  <!-- ✅ Usar melhorias do component.ts -->
  <span class="status">{{ getCurrentStatus() }}</span>
  <div [class.active]="isActiveState()">Content</div>
</div>

<!-- ❌ ERRADO - Alterar estrutura HTML -->
<div class="existing-class">
  <fieldset> <!-- ❌ NÃO adicionar novos elementos -->
    <legend>Campo</legend> <!-- ❌ NÃO adicionar novos elementos -->
    <p>{{ userDisplayName() }}</p>
  </fieldset> <!-- ❌ NÃO adicionar novos elementos -->
</div>

<!-- ❌ ERRADO - Remover ou mover elementos -->
<!-- Remover <p> ou mover para outro lugar -->
```

### Tipos de Referências Permitidas do Component.ts

```html
<!-- ✅ SIGNALS -->
<p>{{ userName() }}</p>
<div *ngIf="isLoading()">Loading...</div>

<!-- ✅ COMPUTED PROPERTIES -->
<span>{{ fullName() }}</span>
<img [src]="avatarUrl()" />

<!-- ✅ GETTERS -->
<div>{{ get errorMessage() }}</div>
<button [disabled]="get isFormValid">Submit</button>

<!-- ✅ MÉTODOS PÚBLICOS/PROTECTED -->
<div (click)="handleClick()">{{ getDisplayText() }}</div>
<span>{{ formatCurrency(amount) }}</span>

<!-- ✅ PROPRIEDADES CALCULADAS -->
<div [style.color]="themeColor">{{ dynamicContent }}</div>

<!-- ✅ COMBINAÇÕES -->
<input 
  [value]="inputValue()" 
  (input)="updateValue($event)"
  [attr.aria-label]="getInputLabel()"
  [class.error]="hasInputError()"
/>
```

### Acessibilidade sem Alterar Estrutura

```html
<!-- ✅ EXEMPLOS PERMITIDOS -->

<!-- Adicionar atributos ARIA -->
<button [attr.aria-label]="buttonDescription()">Salvar</button>

<!-- Adicionar roles -->
<div role="alert" *ngIf="showError()">Erro</div>

<!-- Adicionar tipos de input -->
<input type="tel" formControlName="phone" />

<!-- Adicionar referências para acessibilidade -->
<label id="email-label">Email</label>
<input [attr.aria-labelledby]="'email-label'" />

<!-- Adicionar estados dinâmicos -->
<div [attr.aria-live]="hasChanges() ? 'polite' : null">
  Conteúdo dinâmico
</div>
```

## 4. Reactive Programming & RxJS

### Observable Best Practices

```typescript
// ✅ Usar async pipe sempre que possível
@Component({
  template: `
    <div *ngIf="user$ | async as user">
      {{ user.name }}
    </div>
    <ul>
      <li *ngFor="let item of items$ | async">{{ item.name }}</li>
    </ul>
  `
})
export class UserComponent {
  protected user$ = this.userService.getUser();
  protected items$ = this.itemService.getItems();

  private userService = inject(UserService);
  private itemService = inject(ItemService);
}

// ✅ Usar takeUntilDestroyed para unsubscribe automático
export class MyComponent implements OnInit {
  private destroyRef = inject(DestroyRef);
  private dataService = inject(DataService);

  ngOnInit(): void {
    this.dataService.getData()
      .pipe(
        takeUntilDestroyed(this.destroyRef),
        map(data => this.transformData(data)),
        catchError(error => this.handleError(error))
      )
      .subscribe(data => this.processData(data));
  }
}

// ✅ Error handling com catchError
private loadData(): Observable<Data[]> {
  return this.http.get<Data[]>('/api/data').pipe(
    retry(3),
    catchError(error => {
      console.error('Erro ao carregar dados:', error);
      return of([]); // Fallback
    })
  );
}
```

### Subjects e BehaviorSubjects

```typescript
// ✅ Usar signals ao invés de Subjects quando possível
@Injectable()
export class StateService {
  // ✅ PREFERIR - Signals
  private _users = signal<User[]>([]);
  readonly users = this._users.asReadonly();

  addUser(user: User): void {
    this._users.update((users) => [...users, user]);
  }
}

// ✅ Quando precisar de Subjects, use tipagem forte
@Injectable()
export class NotificationService {
  private notificationSubject = new Subject<Notification>();

  readonly notifications$ = this.notificationSubject.asObservable();

  notify(message: string, type: NotificationType = 'info'): void {
    this.notificationSubject.next({ message, type, timestamp: Date.now() });
  }
}
```

## 5. Forms & Validation

### Reactive Forms com Signals

```typescript
// ✅ Forms reativas com tipagem forte
interface UserForm {
  name: string;
  email: string;
  age: number;
}

@Component({
  selector: 'app-user-form',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="userForm" (ngSubmit)="onSubmit()">
      <input formControlName="name" placeholder="Nome" />
      <div *ngIf="nameControl.invalid && nameControl.touched" class="error">Nome é obrigatório</div>

      <input formControlName="email" type="email" placeholder="Email" />
      <div *ngIf="emailControl.invalid && emailControl.touched" class="error">Email deve ser válido</div>

      <button type="submit" [disabled]="userForm.invalid || isSubmitting()">
        {{ isSubmitting() ? 'Salvando...' : 'Salvar' }}
      </button>
    </form>
  `,
})
export class UserFormComponent {
  private fb = inject(FormBuilder);

  protected isSubmitting = signal(false);

  protected userForm = this.fb.group({
    name: ['', [Validators.required, Validators.minLength(2)]],
    email: ['', [Validators.required, Validators.email]],
    age: [null, [Validators.required, Validators.min(18)]],
  });

  // ✅ Getters para fácil acesso aos controles
  protected get nameControl() {
    return this.userForm.get('name')!;
  }
  protected get emailControl() {
    return this.userForm.get('email')!;
  }

  protected async onSubmit(): Promise<void> {
    if (this.userForm.valid) {
      this.isSubmitting.set(true);
      try {
        const formValue = this.userForm.value as UserForm;
        await this.saveUser(formValue);
      } finally {
        this.isSubmitting.set(false);
      }
    }
  }

  private async saveUser(user: UserForm): Promise<void> {
    // Implementar salvamento
  }
}
```

### Custom Validators

```typescript
// ✅ Validators customizados reutilizáveis
export class CustomValidators {
  static cpf(control: AbstractControl): ValidationErrors | null {
    const value = control.value;
    if (!value) return null;

    const isValid = validateCPF(value);
    return isValid ? null : { cpf: { value } };
  }

  static passwordStrength(control: AbstractControl): ValidationErrors | null {
    const value = control.value;
    if (!value) return null;

    const hasNumber = /[0-9]/.test(value);
    const hasUpper = /[A-Z]/.test(value);
    const hasLower = /[a-z]/.test(value);
    const hasSpecial = /[!@#$%^&*(),.?":{}|<>]/.test(value);

    const valid = hasNumber && hasUpper && hasLower && hasSpecial && value.length >= 8;

    return valid
      ? null
      : {
          passwordStrength: {
            hasNumber,
            hasUpper,
            hasLower,
            hasSpecial,
            minLength: value.length >= 8,
          },
        };
  }
}
```

## 6. Services & HTTP

### Service Design Patterns

```typescript
// ✅ Service com estado usando signals
@Injectable({
  providedIn: 'root',
})
export class UserService {
  private http = inject(HttpClient);

  // ✅ Estado reativo com signals
  private _users = signal<User[]>([]);
  private _loading = signal(false);
  private _error = signal<string | null>(null);

  // ✅ Readonly getters
  readonly users = this._users.asReadonly();
  readonly loading = this._loading.asReadonly();
  readonly error = this._error.asReadonly();

  async loadUsers(): Promise<void> {
    this._loading.set(true);
    this._error.set(null);

    try {
      const users = await firstValueFrom(this.http.get<User[]>('/api/users').pipe(retry(3), timeout(10000)));
      this._users.set(users);
    } catch (error) {
      const message = error instanceof Error ? error.message : 'Erro desconhecido';
      this._error.set(message);
      console.error('Erro ao carregar usuários:', error);
    } finally {
      this._loading.set(false);
    }
  }

  async createUser(userData: CreateUserRequest): Promise<User> {
    const user = await firstValueFrom(this.http.post<User>('/api/users', userData));

    this._users.update((users) => [...users, user]);
    return user;
  }
}
```

### HTTP Interceptors Modernos

```typescript
// ✅ Interceptor funcional (Angular 15+)
export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const authToken = inject(AuthService).getToken();

  if (authToken) {
    const authReq = req.clone({
      headers: req.headers.set('Authorization', `Bearer ${authToken}`),
    });
    return next(authReq);
  }

  return next(req);
};

// ✅ Error interceptor
export const errorInterceptor: HttpInterceptorFn = (req, next) => {
  return next(req).pipe(
    catchError((error: HttpErrorResponse) => {
      if (error.status === 401) {
        inject(AuthService).logout();
        inject(Router).navigate(['/login']);
      }

      const notification = inject(NotificationService);
      notification.showError(getErrorMessage(error));

      return throwError(() => error);
    }),
  );
};

// ✅ Configuração no main.ts
bootstrapApplication(AppComponent, {
  providers: [provideHttpClient(withInterceptors([authInterceptor, errorInterceptor]))],
});
```

## 7. Security Best Practices

### XSS Prevention

```typescript
// ✅ SEMPRE sanitizar entradas do usuário
@Component({
  template: `
    <!-- ✅ Interpolação é automaticamente escapada -->
    <p>{{ userInput }}</p>

    <!-- ⚠️ innerHTML requer cuidado -->
    <div [innerHTML]="trustedHtml"></div>

    <!-- ✅ Usar DomSanitizer quando necessário -->
    <iframe [src]="trustedVideoUrl"></iframe>
  `,
})
export class SafeComponent {
  private sanitizer = inject(DomSanitizer);

  userInput = 'Conteúdo do usuário'; // Automaticamente escapado

  // ✅ Sanitizar HTML quando necessário
  get trustedHtml(): SafeHtml {
    const rawHtml = '<b>Conteúdo seguro</b>';
    return this.sanitizer.bypassSecurityTrustHtml(rawHtml);
  }

  // ✅ URLs de recursos confiáveis
  get trustedVideoUrl(): SafeResourceUrl {
    const videoId = 'abc123'; // Validar ID
    const url = `https://www.youtube.com/embed/${videoId}`;
    return this.sanitizer.bypassSecurityTrustResourceUrl(url);
  }
}
```

### CSRF Protection

```typescript
// ✅ Configurar XSRF protection
bootstrapApplication(AppComponent, {
  providers: [
    provideHttpClient(
      withXsrfConfiguration({
        cookieName: 'XSRF-TOKEN',
        headerName: 'X-XSRF-TOKEN',
      }),
    ),
  ],
});

// ✅ Interceptor para CSRF token
export const csrfInterceptor: HttpInterceptorFn = (req, next) => {
  // Angular automaticamente adiciona XSRF token para requests mutáveis
  return next(req);
};
```

### Content Security Policy

```html
<!-- ✅ CSP Headers recomendados -->
<meta
  http-equiv="Content-Security-Policy"
  content="default-src 'self'; 
               style-src 'self' 'unsafe-inline'; 
               script-src 'self' 'nonce-randomValue';"
/>
```

## 8. Testing Best Practices

### Component Testing

```typescript
// ✅ Testes usando TestBed com signals
describe('UserComponent', () => {
  let component: UserComponent;
  let fixture: ComponentFixture<UserComponent>;
  let userService: jasmine.SpyObj<UserService>;

  beforeEach(async () => {
    const userServiceSpy = jasmine.createSpyObj('UserService', ['getUser', 'updateUser']);

    await TestBed.configureTestingModule({
      imports: [UserComponent], // Standalone component
      providers: [{ provide: UserService, useValue: userServiceSpy }],
    }).compileComponents();

    userService = TestBed.inject(UserService) as jasmine.SpyObj<UserService>;
    fixture = TestBed.createComponent(UserComponent);
    component = fixture.componentInstance;
  });

  it('should display user name when user is provided', () => {
    // ✅ Usar fixture.componentRef.setInput para signal inputs
    fixture.componentRef.setInput('user', mockUser);
    fixture.detectChanges();

    const compiled = fixture.nativeElement as HTMLElement;
    expect(compiled.querySelector('h1')?.textContent).toContain(mockUser.name);
  });

  it('should emit user updated event', () => {
    let emittedUser: User | undefined;
    component.userUpdated.subscribe((user) => (emittedUser = user));

    component.updateUser(mockUser);

    expect(emittedUser).toEqual(mockUser);
  });
});
```

### Service Testing

```typescript
// ✅ Testes de serviços com HttpClientTestingModule
describe('UserService', () => {
  let service: UserService;
  let httpMock: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
    });
    service = TestBed.inject(UserService);
    httpMock = TestBed.inject(HttpTestingController);
  });

  afterEach(() => {
    httpMock.verify(); // ✅ Verificar que não há requests pendentes
  });

  it('should fetch users', async () => {
    const mockUsers: User[] = [{ id: 1, name: 'John' }];

    const promise = service.loadUsers();

    const req = httpMock.expectOne('/api/users');
    expect(req.request.method).toBe('GET');
    req.flush(mockUsers);

    await promise;
    expect(service.users()).toEqual(mockUsers);
  });

  it('should handle errors', async () => {
    const promise = service.loadUsers();

    const req = httpMock.expectOne('/api/users');
    req.error(new ProgressEvent('Network error'));

    await promise;
    expect(service.error()).toBeTruthy();
    expect(service.users()).toEqual([]);
  });
});
```

### E2E Testing

```typescript
// ✅ E2E tests com Playwright/Cypress
describe('User Management', () => {
  it('should create new user', () => {
    cy.visit('/users');
    cy.get('[data-cy=add-user-btn]').click();

    cy.get('[data-cy=name-input]').type('João Silva');
    cy.get('[data-cy=email-input]').type('joao@example.com');
    cy.get('[data-cy=save-btn]').click();

    cy.get('[data-cy=user-list]').should('contain', 'João Silva');
  });
});
```

## 9. Performance Optimization

### Bundle Optimization

```typescript
// ✅ Lazy loading de rotas
const routes: Routes = [
  {
    path: 'users',
    loadComponent: () => import('./features/users/user-list.component').then((m) => m.UserListComponent),
  },
  {
    path: 'admin',
    loadChildren: () => import('./features/admin/admin.routes').then((m) => m.adminRoutes),
  },
];

// ✅ Standalone components para melhor tree-shaking
@Component({
  selector: 'app-user-card',
  standalone: true,
  imports: [CommonModule, MatCardModule],
  changeDetection: ChangeDetectionStrategy.OnPush,
})
export class UserCardComponent {}
```

### Virtual Scrolling para Listas Grandes

```typescript
@Component({
  selector: 'app-virtual-list',
  standalone: true,
  imports: [ScrollingModule],
  template: `
    <cdk-virtual-scroll-viewport itemSize="50" class="viewport">
      <div *cdkVirtualFor="let item of items(); trackBy: trackByItem">
        {{ item.name }}
      </div>
    </cdk-virtual-scroll-viewport>
  `,
  styles: [
    `
      .viewport {
        height: 400px;
      }
    `,
  ],
})
export class VirtualListComponent {
  readonly items = input.required<Item[]>();

  protected trackByItem(index: number, item: Item): number {
    return item.id;
  }
}
```

### Image Optimization

```html
<!-- ✅ NgOptimizedImage para otimização automática -->
<img ngSrc="/assets/hero-image.jpg" alt="Hero Image" width="800" height="600" priority />

<!-- ✅ Lazy loading para imagens não críticas -->
<img ngSrc="/assets/gallery-image.jpg" alt="Gallery Image" width="400" height="300" loading="lazy" />
```

### Preloading Strategies

```typescript
// ✅ Estratégia de preload customizada
@Injectable()
export class CustomPreloadingStrategy implements PreloadingStrategy {
  preload(route: Route, fn: () => Observable<any>): Observable<any> {
    // Preload apenas rotas marcadas
    return route.data?.['preload'] ? fn() : of(null);
  }
}

// ✅ Configuração no main.ts
bootstrapApplication(AppComponent, {
  providers: [provideRouter(routes, withPreloading(CustomPreloadingStrategy))],
});
```

## 10. Accessibility & UX

### ARIA Attributes

```html
<!-- ✅ Usar attr. prefix para ARIA attributes -->
<button [attr.aria-label]="buttonLabel()">
  <mat-icon>delete</mat-icon>
</button>

<!-- ✅ Atributos ARIA estáticos -->
<div role="tabpanel" aria-labelledby="tab-1">Conteúdo do painel</div>

<!-- ✅ Progress bar acessível -->
<div role="progressbar" [attr.aria-valuenow]="progress()" aria-valuemin="0" aria-valuemax="100">{{ progress() }}%</div>
```

### Focus Management

```typescript
// ✅ Gerenciar foco após navegação
@Injectable()
export class FocusService {
  private router = inject(Router);

  constructor() {
    this.router.events.pipe(filter((event) => event instanceof NavigationEnd)).subscribe(() => {
      this.focusMainContent();
    });
  }

  private focusMainContent(): void {
    const mainContent = document.querySelector('[role="main"]') as HTMLElement;
    if (mainContent) {
      mainContent.focus();
    }
  }
}
```

### Keyboard Navigation

```typescript
@Component({
  selector: 'app-menu',
  template: `
    <nav role="navigation">
      <a *ngFor="let item of menuItems(); trackBy: trackByItem" [routerLink]="item.path" routerLinkActive="active" ariaCurrentWhenActive="page" (keydown)="onKeyDown($event)">
        {{ item.label }}
      </a>
    </nav>
  `,
})
export class MenuComponent {
  readonly menuItems = input.required<MenuItem[]>();

  protected onKeyDown(event: KeyboardEvent): void {
    const target = event.target as HTMLElement;

    switch (event.key) {
      case 'ArrowDown':
        this.focusNext(target);
        event.preventDefault();
        break;
      case 'ArrowUp':
        this.focusPrevious(target);
        event.preventDefault();
        break;
    }
  }

  private focusNext(current: HTMLElement): void {
    const next = current.nextElementSibling as HTMLElement;
    next?.focus();
  }

  private focusPrevious(current: HTMLElement): void {
    const previous = current.previousElementSibling as HTMLElement;
    previous?.focus();
  }

  protected trackByItem(index: number, item: MenuItem): string {
    return item.id;
  }
}
```

## 11. Error Handling & Zone Management

### NgZone Optimization

```typescript
// ✅ Evitar zone pollution com bibliotecas externas
@Component({
  selector: 'app-chart',
  standalone: true,
  template: '<div #chartContainer></div>',
})
export class ChartComponent implements OnInit {
  private ngZone = inject(NgZone);
  private chartContainer = viewChild.required<ElementRef>('chartContainer');

  ngOnInit(): void {
    // ✅ Executar bibliotecas externas fora da zone
    this.ngZone.runOutsideAngular(() => {
      this.initializeChart();
    });
  }

  private initializeChart(): void {
    // ✅ Bibliotecas como Plotly, Chart.js, etc. fora da Angular zone
    const chart = new Chart(this.chartContainer().nativeElement, {
      type: 'line',
      data: {
        labels: ['Jan', 'Feb', 'Mar'],
        datasets: [
          {
            label: 'Vendas',
            data: [12, 19, 3],
          },
        ],
      },
    });

    // ✅ Re-entrar na zone apenas quando necessário
    chart.on('click', (event) => {
      this.ngZone.run(() => {
        this.handleChartClick(event);
      });
    });
  }

  private handleChartClick(event: any): void {
    // Lógica que precisa de change detection
  }
}

// ❌ Evitar - executar bibliotecas dentro da zone
@Component({
  template: '<div #chart></div>',
})
export class BadChartComponent {
  ngOnInit(): void {
    // ❌ Causa change detection desnecessário
    new Chart(this.chart.nativeElement, config);
  }
}
```

### Performance Monitoring

```typescript
// ✅ Monitorar performance com Angular DevTools
@Component({
  selector: 'app-performance-monitor',
  template: `
    <div>
      <p>Change Detection Cycles: {{ changeDetectionCount() }}</p>
      <p>Render Time: {{ renderTime() }}ms</p>
      <button (click)="triggerUpdate()">Test Performance</button>
    </div>
  `,
})
export class PerformanceMonitorComponent implements OnInit {
  protected changeDetectionCount = signal(0);
  protected renderTime = signal(0);

  ngOnInit(): void {
    // ✅ Habilitar profiling em desenvolvimento
    if (isDevMode()) {
      this.enablePerformanceMonitoring();
    }
  }

  ngDoCheck(): void {
    this.changeDetectionCount.update((count) => count + 1);
  }

  protected triggerUpdate(): void {
    const start = performance.now();
    this.heavyComputation();
    const end = performance.now();
    this.renderTime.set(Math.round(end - start));
  }

  private enablePerformanceMonitoring(): void {
    // ✅ Angular DevTools integration
    import('@angular/core').then(({ enableProfiling }) => {
      enableProfiling();
    });
  }

  private heavyComputation(): void {
    // Simular cálculo pesado para teste
    for (let i = 0; i < 1000000; i++) {
      Math.random();
    }
  }
}
```

### Global Error Handler

```typescript
// ✅ Error handler personalizado
@Injectable({
  providedIn: 'root',
})
export class GlobalErrorHandler implements ErrorHandler {
  private notification = inject(NotificationService);
  private logger = inject(LoggerService);

  handleError(error: any): void {
    console.error('Global error caught:', error);

    // ✅ Log estruturado
    this.logError(error);

    // ✅ Notificar usuário de forma amigável
    this.notifyUser(error);

    // ✅ Reportar para monitoramento externo
    this.reportToMonitoring(error);
  }

  private logError(error: any): void {
    const errorInfo = {
      message: error.message,
      stack: error.stack,
      url: window.location.href,
      userAgent: navigator.userAgent,
      timestamp: new Date().toISOString(),
    };

    this.logger.error('Application Error', errorInfo);
  }

  private notifyUser(error: any): void {
    const userMessage = this.getUserFriendlyMessage(error);
    this.notification.showError(userMessage);
  }

  private getUserFriendlyMessage(error: any): string {
    // ✅ Mapear erros técnicos para mensagens amigáveis
    if (error.name === 'ChunkLoadError') {
      return 'Nova versão disponível. Recarregue a página.';
    }

    if (error.status === 0) {
      return 'Problema de conexão. Verifique sua internet.';
    }

    if (error.status >= 500) {
      return 'Erro interno do servidor. Tente novamente em alguns minutos.';
    }

    return 'Ocorreu um erro inesperado. Nossa equipe foi notificada.';
  }

  private reportToMonitoring(error: any): void {
    // ✅ Integração com Sentry, LogRocket, etc.
    if (typeof Sentry !== 'undefined') {
      Sentry.captureException(error);
    }
  }
}

// ✅ Configuração no main.ts
bootstrapApplication(AppComponent, {
  providers: [{ provide: ErrorHandler, useClass: GlobalErrorHandler }, provideBrowserGlobalErrorListeners()],
});
```

### Optimizing Slow Computations

```typescript
// ✅ Usar computed signals para cálculos pesados
@Component({
  selector: 'app-data-processor',
  template: `
    <div>
      <p>Processing {{ items().length }} items...</p>
      <p>Result: {{ processedData() }}</p>
      <p>Computation time: {{ computationTime() }}ms</p>
    </div>
  `,
})
export class DataProcessorComponent {
  readonly items = input.required<DataItem[]>();

  // ✅ Computed para cache automático
  protected processedData = computed(() => {
    const start = performance.now();
    const result = this.processItems(this.items());
    const end = performance.now();

    this.computationTime.set(Math.round(end - start));
    return result;
  });

  private computationTime = signal(0);

  private processItems(items: DataItem[]): ProcessedData {
    // ✅ Otimizar algoritmo primeiro
    return items
      .filter((item) => item.isActive)
      .map((item) => this.transformItem(item))
      .reduce((acc, item) => ({ ...acc, ...item }), {});
  }

  private transformItem(item: DataItem): any {
    return {
      [item.id]: item.value * 2,
    };
  }
}

// ✅ Pure pipe para cálculos reutilizáveis
@Pipe({
  name: 'expensiveCalculation',
  pure: true, // ✅ Cache automático quando inputs não mudam
  standalone: true,
})
export class ExpensiveCalculationPipe implements PipeTransform {
  transform(data: any[], factor: number): any[] {
    console.log('Pipe executado - dados sendo processados');

    return data.map((item) => ({
      ...item,
      calculated: item.value * factor,
    }));
  }
}

// ❌ Evitar - cálculos pesados em templates
@Component({
  template: `
    <!-- ❌ Executa a cada change detection -->
    <div>{{ heavyComputation(data) }}</div>
  `,
})
export class BadComputationComponent {
  heavyComputation(data: any[]): any {
    // ❌ Processamento pesado executado sempre
    return data.reduce((acc, item) => acc + item.value, 0);
  }
}
```

### OnPush Change Detection Strategy

```typescript
// ✅ OnPush para otimizar performance
@Component({
  selector: 'app-optimized-list',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `
    <div>
      <h3>Items ({{ items().length }})</h3>
      <div *ngFor="let item of items(); trackBy: trackByItem">{{ item.name }} - {{ item.value }}</div>
      <button (click)="addItem()">Add Item</button>
    </div>
  `,
})
export class OptimizedListComponent {
  // ✅ Signal inputs para OnPush
  readonly items = input.required<Item[]>();

  // ✅ Output signals para eventos
  readonly itemAdded = output<Item>();

  protected trackByItem(index: number, item: Item): string {
    return item.id;
  }

  protected addItem(): void {
    const newItem: Item = {
      id: crypto.randomUUID(),
      name: `Item ${Date.now()}`,
      value: Math.random(),
    };

    // ✅ Emit através de output signal
    this.itemAdded.emit(newItem);
  }
}

// ✅ Parent component gerencia estado
@Component({
  selector: 'app-parent',
  template: ` <app-optimized-list [items]="items()" (itemAdded)="onItemAdded($event)" /> `,
})
export class ParentComponent {
  protected items = signal<Item[]>([]);

  protected onItemAdded(item: Item): void {
    // ✅ Atualização imutável dispara change detection
    this.items.update((current) => [...current, item]);
  }
}
```

### Zoneless Change Detection (Future-Ready)

```typescript
// ✅ Preparação para Angular Zoneless (Angular 18+)
@Component({
  selector: 'app-zoneless-ready',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush, // ✅ Obrigatório
  template: `
    <div>
      <p>Count: {{ count() }}</p>
      <p>Async Data: {{ asyncData() | async }}</p>
      <button (click)="increment()">Increment</button>
    </div>
  `,
})
export class ZonelessReadyComponent {
  // ✅ Usar signals para estado reativo
  protected count = signal(0);

  // ✅ Async pipe para observables
  protected asyncData = signal(of('Loading...'));

  private dataService = inject(DataService);

  ngOnInit(): void {
    // ✅ Padrão compatível com zoneless
    this.loadAsyncData();
  }

  protected increment(): void {
    // ✅ Signals disparam change detection automaticamente
    this.count.update((val) => val + 1);
  }

  private loadAsyncData(): void {
    // ✅ Async pipe handle change detection
    this.asyncData.set(this.dataService.getData().pipe(catchError((error) => of('Error loading data'))));
  }
}

// ✅ Bootstrap para ambiente Zoneless (quando disponível)
/*
bootstrapApplication(AppComponent, {
  providers: [
    // Experimental - Angular 18+
    // provideZonelessChangeDetection()
  ]
});
*/
```

## 12. Bundle Optimization & Lazy Loading

### Lazy Loading Modules

```typescript
// ✅ Lazy loading com standalone components
const routes: Routes = [
  {
    path: 'admin',
    loadComponent: () => import('./admin/admin.component').then((m) => m.AdminComponent),
    canMatch: [AdminGuard],
  },
  {
    path: 'products',
    loadChildren: () => import('./products/products.routes').then((m) => m.PRODUCT_ROUTES),
  },
  {
    path: 'settings',
    loadComponent: () => import('./settings/settings.component'),
    providers: [SettingsService], // ✅ Providers específicos da rota
  },
];

// ✅ products.routes.ts
export const PRODUCT_ROUTES: Routes = [
  {
    path: '',
    loadComponent: () => import('./product-list/product-list.component'),
  },
  {
    path: ':id',
    loadComponent: () => import('./product-detail/product-detail.component'),
  },
];
```

### Code Splitting Strategies

```typescript
// ✅ Dynamic imports para bibliotecas grandes
@Component({
  selector: 'app-chart-viewer',
  template: `
    <div>
      <button (click)="loadChart()" [disabled]="loading()">
        {{ loading() ? 'Loading...' : 'Load Chart' }}
      </button>
      <div #chartContainer></div>
    </div>
  `,
})
export class ChartViewerComponent {
  private chartContainer = viewChild.required<ElementRef>('chartContainer');
  protected loading = signal(false);

  protected async loadChart(): Promise<void> {
    this.loading.set(true);

    try {
      // ✅ Import dinâmico - chunk separado
      const [{ Chart }, chartData] = await Promise.all([import('chart.js/auto'), import('./chart-config').then((m) => m.chartData)]);

      new Chart(this.chartContainer().nativeElement, chartData);
    } catch (error) {
      console.error('Failed to load chart:', error);
    } finally {
      this.loading.set(false);
    }
  }
}

// ✅ Preload strategies
@NgModule({
  imports: [
    RouterModule.forRoot(routes, {
      preloadingStrategy: PreloadAllModules, // ou QuicklinkStrategy
      enableTracing: false, // ✅ Só em desenvolvimento
    }),
  ],
})
export class AppRoutingModule {}
```

### Tree Shaking Optimization

```typescript
// ✅ Imports específicos para tree shaking
import { map, filter, switchMap } from 'rxjs/operators';
import { Observable } from 'rxjs;

// ❌ Evitar - import completo da biblioteca
// import * as _ from 'lodash';
// import { rxjs } from 'rxjs';

// ✅ Use barrel exports seletivos
// shared/index.ts
export { UserService } from './user.service';
export { DataService } from './data.service';
// Não exportar tudo com export *

// ✅ Conditional imports
@Injectable()
export class AnalyticsService {
  async trackEvent(event: string, data: any): Promise<void> {
    if (environment.production) {
      // ✅ Só carrega analytics em produção
      const { gtag } = await import('./gtag');
      gtag('event', event, data);
    }
  }
}
```

### Bundle Analysis

```typescript
// ✅ Webpack Bundle Analyzer configuration
// angular.json
{
  "build": {
    "configurations": {
      "analyze": {
        "budgets": [
          {
            "type": "initial",
            "maximumWarning": "2mb",
            "maximumError": "5mb"
          }
        ],
        "statsJson": true
      }
    }
  }
}

// ✅ Script no package.json
{
  "scripts": {
    "analyze": "ng build --configuration analyze && npx webpack-bundle-analyzer dist/app/stats.json"
  }
}

// ✅ Performance budgets
// angular.json budgets
[
  {
    "type": "bundle",
    "name": "main",
    "baseline": "2mb",
    "maximumWarning": "2mb",
    "maximumError": "2.5mb"
  },
  {
    "type": "anyComponentStyle",
    "maximumWarning": "6kb"
  }
]
```
