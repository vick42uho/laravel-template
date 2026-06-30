# คู่มือ Laravel สำหรับโปรเจกต์นี้ (สำหรับผู้เริ่มต้น)

โปรเจกต์นี้ใช้ **Laravel 13** เป็น Backend, **Vue 3** เป็น Frontend และเชื่อมต่อกันด้วย **Inertia.js** (ทำให้เขียน SPA โดยไม่ต้องทำ API แยก)

---

## คำสั่งรันโปรเจกต์ (สำคัญ! ต้องรู้)

### ติดตั้ง Dependencies (ครั้งแรก หรือเมื่อมีอัปเดต)

```powershell
# ติดตั้ง PHP dependencies
C:\php\php.exe C:\Users\it-dev\.config\herd-lite\bin\composer.phar install

# ติดตั้ง JavaScript dependencies (ใช้ yarn registry เพราะ npmjs.org ถูกบล็อค)
npm install --registry=https://registry.yarnpkg.com

# Build frontend สำหรับ production
npm run build
```

### รัน Development Server

เปิด **2 terminals** แยกกัน:

**Terminal 1 — Laravel Server:**
```powershell
cd d:\My-work_My-Everything\diretory-app
C:\php\php.exe artisan serve --host=localhost
```
→ เปิด http://localhost:8000

**Terminal 2 — Vite Dev Server (Frontend):**
```powershell
cd d:\My-work_My-Everything\diretory-app
npm run dev
```
→ Vite จะทำงานที่ http://localhost:5173 (ไม่ต้องเปิดเอง) 

> **หมายเหตุ:** ห้ามใช้ `composer run dev` บน Windows เพราะ Laravel Pail ต้องการ `pcntl` extension ซึ่งไม่มีบน Windows

### Database Commands

```powershell
# รัน migrations ทั้งหมด (สร้างตาราง)
C:\php\php.exe artisan migrate

# ย้อนกลับ migration ล่าสุด 1 ครั้ง
C:\php\php.exe artisan migrate:rollback

# ย้อนกลับทั้งหมด แล้วรันใหม่ (ระวัง! ข้อมูลหาย)
C:\php\php.exe artisan migrate:refresh

# ลบข้อมูลทุกตารางแล้ว migrate ใหม่ (ใช้ตอน dev)
C:\php\php.exe artisan migrate:fresh

# ใส่ข้อมูลตัวอย่าง (seeder)
C:\php\php.exe artisan db:seed

# ลบข้อมูลทั้งหมด + migrate ใหม่ + seed
C:\php\php.exe artisan migrate:fresh --seed
```

### Utility Commands

```powershell
# เคลียร์ cache ต่างๆ (ควรทำเมื่อแก้ .env หรือ config)
C:\php\php.exe artisan config:clear    # เคลียร์ config cache
C:\php\php.exe artisan cache:clear      # เคลียร์ application cache
C:\php\php.exe artisan route:clear      # เคลียร์ route cache
C:\php\php.exe artisan view:clear       # เคลียร์ compiled views

# ดู routes ทั้งหมด (มี URL อะไรบ้าง, ใช้ method อะไร)
C:\php\php.exe artisan route:list

# เปิด interactive PHP shell (ทดสอบ code ทีละบรรทัด)
C:\php\php.exe artisan tinker

# รัน tests
C:\php\php.exe artisan test
C:\php\php.exe artisan test --filter=TeamTest    # รันเฉพาะ class
```

### สร้างไฟล์ด้วย Artisan (Scaffolding)

```powershell
C:\php\php.exe artisan make:model Post           # สร้าง Model
C:\php\php.exe artisan make:controller PostController  # สร้าง Controller
C:\php\php.exe artisan make:controller PostController --resource  # Controller ครบ CRUD
C:\php\php.exe artisan make:migration create_posts_table  # สร้าง migration
C:\php\php.exe artisan make:request StorePostRequest  # สร้าง Form Request (validation)
C:\php\php.exe artisan make:policy PostPolicy    # สร้าง Policy (authorization)
C:\php\php.exe artisan make:middleware CheckAge   # สร้าง Middleware
C:\php\php.exe artisan make:seeder PostSeeder     # สร้าง Seeder
C:\php\php.exe artisan make:test PostTest         # สร้าง test
C:\php\php.exe artisan make:test PostTest --unit  # สร้าง unit test
```

### Frontend Commands

```powershell
npm run dev            # Vite dev server (hot reload)
npm run build          # Build สำหรับ production
npm run format         # จัด format code ด้วย Prettier
npm run format:check   # ตรวจสอบ format
npm run lint           # แก้ lint errors อัตโนมัติ
npm run lint:check     # ตรวจสอบ lint (ไม่แก้)
npm run types:check    # ตรวจสอบ TypeScript types
```

---

## โครงสร้างโฟลเดอร์สำคัญ
diretory-app/
├── app/
│   ├── Http/
│   │   ├── Controllers/    ← ตัวจัดการ request (รับ input → ส่ง response)
│   │   ├── Middleware/      ← ตัวกรอง request ก่อนเข้า controller
│   │   └── Requests/        ← validation rules สำหรับ form input
│   ├── Models/              ← ตัวแทนตารางในฐานข้อมูล
│   ├── Enums/               ← ค่าคงที่แบบกำหนด (เช่น TeamRole::Owner)
│   ├── Policies/            ← กำหนดสิทธิ์ "ใครทำได้/ไม่ได้"
│   └── Providers/           ← ตั้งค่าระบบตอน app เริ่มทำงาน
├── routes/
│   ├── web.php              ← URL routes หลัก
│   └── settings.php         ← routes สำหรับ settings/teams
├── resources/js/
│   ├── pages/               ← หน้า Vue components (หน้าหลัก)
│   ├── components/          ← UI components ย่อยๆ
│   ├── layouts/             ← template/layout ของหน้า
│   └── types/               ← TypeScript types
├── database/
│   └── migrations/          ← ไฟล์สร้าง/แก้ไขตาราง database
├── tests/                   ← ไฟล์ทดสอบ
└── .env                     ← ตั้งค่า environment (DB, keys, etc.)
## การทำงานของ Request (1 request ทำอะไรบ้าง)
User เปิด browser
    ↓
1. routes/web.php          → จับคู่ URL กับ Controller
    ↓
2. Middleware               → ตรวจสอบ auth, team membership, etc.
    ↓
3. Controller               → ดึงข้อมูลจาก Model, ทำ logic
    ↓
4. Inertia::render()        → ส่งข้อมูลไปที่ Vue page
    ↓
5. Vue page (resources/js/pages/) → แสดงผล UI
ตัวอย่างจริงจากโปรเจกต์นี้:
URL: /{team}/dashboard
web.php:
  Route::get('dashboard', DashboardController::class)  // step 1

Middleware: auth + verified + EnsureTeamMembership      // step 2

DashboardController:                                    // step 3
  → ดึง pending invitations ของ user
  → return Inertia::render('Dashboard', [...data])

resources/js/pages/Dashboard.vue                       // step 5
  → แสดงหน้า dashboard + รายการ invitations
## Model คืออะไร?
Model = ตัวแทนตาราง database ใน PHP เช่น Team แทนตาราง teams
// ดึงข้อมูลทั้งหมด
Team::all();

// ดึงพร้อมความสัมพันธ์ (eager loading)
Team::with('members')->find(1);

// สร้างข้อมูลใหม่
Team::create(['name' => 'My Team']);

// ความสัมพันธ์ (relationships)
$team->members;       // สมาชิกในทีม
$team->invitations;   // คำเชิญที่ยังไม่ตอบ
## วิธีเพิ่ม Feature ใหม่ (Step-by-Step)
สมมติจะเพิ่มหน้า "Projects":
Step 1: สร้างตารางใน database
C:\php\php.exe artisan make:migration create_projects_table
แก้ไขไฟล์ migration ที่สร้างใน database/migrations/ แล้ว:
C:\php\php.exe artisan migrate
Step 2: สร้าง Model
C:\php\php.exe artisan make:model Project
แก้ไข app/Models/Project.php เพิ่ม $fillable, ความสัมพันธ์
Step 3: สร้าง Controller
C:\php\php.exe artisan make:controller ProjectController
เพิ่ม method index(), store(), edit(), update(), destroy()
Step 4: เพิ่ม Route
ใน routes/web.php:
Route::resource('projects', ProjectController::class);
Step 5: สร้างหน้า Vue
สร้างไฟล์ resources/js/pages/projects/Index.vue, Edit.vue ฯลฯ
Step 6: ทดสอบ
C:\php\php.exe artisan test
## Eloquent ORM — ใช้งาน Model อย่างละเอียด

Eloquent คือระบบ ORM ของ Laravel ที่ทำให้เราทำงานกับ database ผ่าน PHP object แทน SQL

### สร้าง/แก้ไขข้อมูล

```php
// วิธี 1: สร้างแล้วบันทึก
$post = new Post;
$post->title = 'Hello';
$post->body = 'World';
$post->save();

// วิธี 2: สร้างในบรรทัดเดียว (mass assignment)
$post = Post::create(['title' => 'Hello', 'body' => 'World']);

// แก้ไขข้อมูล
$post->title = 'Updated';
$post->save();

// หรือใช้ update
$post->update(['title' => 'Updated']);
```

### ค้นหาข้อมูล (Query Builder)

```php
// ดึงทั้งหมด
Post::all();

// ดึงตามเงื่อนไข
Post::where('title', 'Hello')->get();
Post::where('views', '>', 100)->orderBy('created_at', 'desc')->get();

// ดึงอันแรก
Post::where('slug', 'hello')->first();

// ดึงตาม ID (error ถ้าไม่มี)
Post::findOrFail(1);

// นับจำนวน
Post::where('published', true)->count();

// ดึงเฉพาะบาง field
Post::select('id', 'title')->get();

// ค้นหาแบบ LIKE
Post::where('title', 'like', '%laravel%')->get();

// หลายเงื่อนไข
Post::where('published', true)
    ->whereIn('category', ['tech', 'design'])
    ->get();
```

### ความสัมพันธ์ (Relationships)

```php
// ใน Model — กำหนดความสัมพันธ์
class Team extends Model {
    public function members(): BelongsToMany {
        return $this->belongsToMany(User::class, 'team_members');
    }
    
    public function invitations(): HasMany {
        return $this->hasMany(TeamInvitation::class);
    }
}

// ใช้งาน
$team = Team::find(1);
$team->members;           // ดึงสมาชิกทั้งหมด
$team->invitations;       // ดึงคำเชิญทั้งหมด
$team->members()->count(); // นับจำนวนสมาชิก

// Eager Loading — ลดจำนวน query (สำคัญมาก!)
// ❌ Bad: query ซ้ำทุก loop
foreach (Team::all() as $team) {
    echo $team->members->count();  // query ใหม่ทุกครั้ง!
}

// ✅ Good: query ครั้งเดียว
foreach (Team::with('members')->get() as $team) {
    echo $team->members->count();  // ใช้ข้อมูลที่โหลดมาแล้ว
}
```

### Soft Deletes

```php
// Model ที่ใช้ SoftDeletes จะไม่ลบจริง แต่ mark deleted_at
class Team extends Model {
    use SoftDeletes;  // ลบแล้วกู้คืนได้
}

$team->delete();          // mark เป็นลบ (deleted_at = now)
$team->restore();         // กู้คืน
$team->forceDelete();     // ลบจริง
Team::withTrashed()->get(); // ดึงทั้งหมดรวมถึงที่ลบแล้ว
```

---

## Form Validation (FormRequest)

**FormRequest** คือ class ที่ใช้ตรวจสอบข้อมูลก่อนเข้า Controller เช่น ตรวจสอบว่า "name ต้องไม่ว่าง" "email ต้องเป็นรูปแบบ email"

### ตัวอย่างจากโปรเจกต์

`app/Http/Requests/Teams/SaveTeamRequest.php`:
```php
class SaveTeamRequest extends FormRequest
{
    public function rules(): array
    {
        return [
            'name' => ['required', 'string', 'max:255', new TeamName],
        ];
    }
}
```

### ใช้ใน Controller
```php
// Laravel จะ validate อัตโนมัติเมื่อรับ FormRequest เป็น parameter
public function store(SaveTeamRequest $request): RedirectResponse
{
    // ถ้า validation ไม่ผ่าน → redirect กลับพร้อม error อัตโนมัติ
    // ถ้าผ่าน → ใช้ $request->validated() ดึงข้อมูลที่ validated แล้ว
    $name = $request->validated('name');
    // ...
}
```

### Validation Rules ที่ใช้บ่อย

| Rule | ความหมาย |
|------|----------|
| `required` | ต้องมีค่า |
| `string` | ต้องเป็น string |
| `max:255` | ยาวได้ไม่เกิน 255 |
| `min:8` | ยาวอย่างน้อย 8 |
| `email` | ต้องเป็นรูปแบบ email |
| `unique:users,email` | ต้องไม่ซ้ำในตาราง users |
| `exists:teams,id` | ต้องมีอยู่ในตาราง teams |
| `integer` | ต้องเป็นตัวเลข |
| `boolean` | ต้องเป็น true/false |
| `nullable` | อนุญาตให้ว่างได้ |
| `confirmed` | ต้องมี field _confirmation ที่ตรงกัน |

---

## Authorization (Gate & Policy)

**Policy** = กำหนดว่า "ใครทำได้/ไม่ได้" สำหรับแต่ละ Model

### ตัวอย่างจากโปรเจกต์ (`app/Policies/TeamPolicy.php`)
```php
class TeamPolicy
{
    // ใคร update team ได้?
    public function update(User $user, Team $team): bool
    {
        return $user->hasTeamPermission($team, TeamPermission::UpdateTeam);
    }
    
    // ใครลบ team ได้?
    public function delete(User $user, Team $team): bool
    {
        return !$team->is_personal 
            && $user->hasTeamPermission($team, TeamPermission::DeleteTeam);
    }
}
```

### ใช้ใน Controller
```php
// ตรวจสอบด้วย Gate
Gate::authorize('update', $team);  // ถ้าไม่ได้ → throw 403

// หรือใช้ใน Route middleware
Route::middleware('can:update,team')->group(function () {
    // ...
});

// ตรวจสอบแบบไม่ throw error
if (Gate::allows('delete', $team)) {
    // อนุญาต
}
```

### ตรวจสอบใน Vue (Frontend)
```vue
<!-- Controller ส่ง permissions มาให้ แล้วใช้ v-if -->
<script setup>
defineProps<{ permissions: { updateTeam: boolean } }>();
</script>

<template>
  <button v-if="permissions.updateTeam">แก้ไขทีม</button>
</template>
```

---

## Middleware — ตัวกรอง Request

Middleware ทำงาน **ก่อน** Controller ใช้สำหรับ: auth, ตรวจสอบสิทธิ์, logging, etc.

### Middleware ในโปรเจกต์นี้

| Middleware | หน้าที่ |
|-----------|--------|
| `auth` | ต้อง login แล้ว |
| `verified` | ต้องยืนยัน email แล้ว |
| `EnsureTeamMembership` | ต้องเป็นสมาชิกของ team นั้น |
| `HandleAppearance` | จัดการ dark/light mode |
| `HandleInertiaRequests` | ส่งข้อมูล shared ไปยัง Vue |
| `SetTeamUrlDefaults` | ตั้ง default team ใน URL |

### สร้าง Middleware ใหม่
```powershell
C:\php\php.exe artisan make:middleware CheckAge
```

```php
class CheckAge
{
    public function handle(Request $request, Closure $next)
    {
        if ($request->user()->age < 18) {
            return redirect('/home');  // ไม่ให้ผ่าน
        }
        return $next($request);  // ให้ผ่าน
    }
}
```

---

## Inertia.js — เชื่อม Laravel กับ Vue

Inertia ทำให้เขียน **Vue SPA** ได้โดยไม่ต้องสร้าง API แยก Controller ส่งข้อมูลผ่าน `Inertia::render()` ตรงๆ

### Backend (Controller)
```php
public function index(Request $request): Response
{
    return Inertia::render('Dashboard', [
        'teams' => $request->user()->teams,
        'pendingInvitations' => TeamInvitation::all(),
    ]);
}
```

### Frontend (Vue page)
```vue
<script setup lang="ts">
import { Head } from '@inertiajs/vue3';
import type { Team, DashboardInvitation } from '@/types';

// รับ props ที่ Controller ส่งมา
defineProps<{
    teams: Team[];
    pendingInvitations?: DashboardInvitation[];
}>();
</script>

<template>
    <Head title="Dashboard" />
    <div v-for="team in teams" :key="team.id">
        {{ team.name }}
    </div>
</template>
```

### ส่งฟอร์มด้วย Inertia
```vue
<script setup>
import { useForm } from '@inertiajs/vue3';

const form = useForm({
    name: '',
    email: '',
});

function submit() {
    form.post('/teams', {
        onSuccess: () => form.reset(),  // ล้างฟอร์มเมื่อสำเร็จ
    });
}
</script>

<template>
    <form @submit.prevent="submit">
        <input v-model="form.name" />
        <div v-if="form.errors.name">{{ form.errors.name }}</div>
        <button type="submit" :disabled="form.processing">บันทึก</button>
    </form>
</template>
```

---

## Testing — การเขียน Test

### รัน Tests
```powershell
C:\php\php.exe artisan test                    # รันทั้งหมด
C:\php\php.exe artisan test --filter=TeamTest  # เฉพาะ class
C:\php\php.exe artisan test tests/Feature       # เฉพาะโฟลเดอร์
```

### เขียน Feature Test (ทดสอบ full request)
```php
class TeamTest extends TestCase
{
    public function test_user_can_create_team(): void
    {
        $user = User::factory()->create();

        $response = $this->actingAs($user)->post('/settings/teams', [
            'name' => 'My Team',
        ]);

        $response->assertRedirect();
        $this->assertDatabaseHas('teams', ['name' => 'My Team']);
    }

    public function test_guest_cannot_create_team(): void
    {
        $response = $this->post('/settings/teams', ['name' => 'My Team']);
        $response->assertRedirect('/login');
    }
}
```

### ใช้ Factory สร้างข้อมูลทดสอบ
```php
// สร้าง user 1 คน
$user = User::factory()->create();

// สร้าง user 5 คน
$users = User::factory()->count(5)->create();

// สร้าง user พร้อม team
$user = User::factory()->has(Team::factory())->create();
```

---

## TypeScript Types (Frontend)

โปรเจกต์นี้ใช้ TypeScript types จาก `resources/js/types/` เพื่อความปลอดภัย

```typescript
// resources/js/types/teams.ts — ตัวอย่าง types
export interface Team {
    id: number;
    name: string;
    slug: string;
    isPersonal: boolean;
}

// ใช้ใน Vue component
defineProps<{
    team: Team;  // TypeScript จะตรวจว่า team ต้องมี field อะไรบ้าง
}>();
```

---

## สิ่งที่ควรรู้เพิ่มเติม

### Service Container & Dependency Injection
```php
// Laravel "ฉีด" dependencies ให้โดยอัตโนมัติ
class TeamController extends Controller
{
    // SaveTeamRequest จะถูก validate อัตโนมัติ
    // CreateTeam action จะถูกสร้างและส่งมาให้
    public function store(SaveTeamRequest $request, CreateTeam $createTeam)
    {
        $createTeam->handle($request->user(), $request->validated('name'));
    }
}
```

### Route Model Binding
```php
// Laravel หา Model จาก URL parameter อัตโนมัติ
// URL: /settings/teams/{team}  →  Laravel หา Team จาก slug
Route::get('settings/teams/{team}', [TeamController::class, 'edit']);

public function edit(Request $request, Team $team): Response
{
    // $team ถูกหาให้แล้ว! ไม่ต้อง Team::find() เอง
    return Inertia::render('teams/Edit', ['team' => $team]);
}
```

### Database Transactions
```php
// ใช้เมื่อต้องบันทึกหลายตาราง — ถ้าอันหนึ่งล้มเหลว ทุกอันย้อนกลับ
DB::transaction(function () {
    $team->update(['name' => 'New Name']);
    $team->members()->detach();
    $team->invitations()->delete();
});
```

### Flash Messages (Toast Notifications)
```php
// ใน Controller
Inertia::flash('toast', [
    'type' => 'success', 
    'message' => 'บันทึกสำเร็จ!'
]);
return to_route('teams.edit', $team);
```

---

## สรุปคำสั่งทั้งหมด (Quick Reference)

### 🚀 รันโปรเจกต์
```powershell
C:\php\php.exe artisan serve --host=localhost  # Laravel server
npm run dev                                      # Vite frontend
```

### 🗄️ Database
```powershell
C:\php\php.exe artisan migrate                 # สร้าง/อัปเดตตาราง
C:\php\php.exe artisan migrate:fresh --seed    # สร้างใหม่ทั้งหมด
```

### 🛠️ สร้างไฟล์
```powershell
C:\php\php.exe artisan make:model Post
C:\php\php.exe artisan make:controller PostController --resource
C:\php\php.exe artisan make:migration create_posts_table
C:\php\php.exe artisan make:request StorePostRequest
C:\php\php.exe artisan make:policy PostPolicy
C:\php\php.exe artisan make:test PostTest
```

### 🔧 ดูแลรักษา
```powershell
C:\php\php.exe artisan config:clear            # เคลียร์ cache
C:\php\php.exe artisan route:list              # ดู routes
C:\php\php.exe artisan test                    # รัน tests
```

---

## สิ่งที่ต้องจำ

- **PHP ใช้ `C:\php\php.exe`** เสมอ (ไม่ใช่ `php` เฉยๆ เพราะ Herd-lite ไม่มี pgsql)
- **Frontend** ใช้ Vue 3 + TypeScript + Tailwind CSS + Inertia.js
- **Database** คือ PostgreSQL ที่ `10.200.11.2:5432/diretory_app`
- **เมื่อแก้ `.env`** ต้องรัน `C:\php\php.exe artisan config:clear` ก่อน
- **Route ทุกหน้าที่ต้อง login** ต้องมี middleware `auth`
- **ใช้ `$request->validated()`** แทน `$request->all()` เสมอ
- **ใช้ `Gate::authorize()`** ก่อนแก้ไข/ลบข้อมูลสำคัญ
- **ใช้ Eager Loading** (`with()`) เมื่อจะ query ความสัมพันธ์ใน loop
