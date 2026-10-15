hãy follow theo https://kiro.dev/docs/steering/ và update lại các steering cho phù hợp với chức năng của có.
chúng tâ

Tình trạng hiện tại:
Project Angular đã có rồi:
src/
├── app/
│ ├── core/ # Singleton services, guards, interceptors
│ │ ├── auth/ # Authentication logic
│ │ ├── guards/ # Route guards
│ │ ├── http/ # HTTP services and types
│ │ ├── interceptors/ # HTTP interceptors
│ │ └── core.providers.ts # Core-level providers
│ │
│ ├── shared/ # Reusable components, directives, pipes
│ │ ├── components/ # Shared UI components
│ │ ├── directives/ # Shared directives
│ │ ├── pipes/ # Shared pipes
│ │ └── utils/ # Utility functions
│ │
│ ├── layout/ # Layout components (shell, nav, etc.)
│ │
│ ├── app.component.ts # Root component
│ ├── app.config.ts # Application configuration
│ ├── app.routes.ts # Route definitions
│ └── app.html # Root template
│
├── assets/ # Static assets (images, fonts, etc.)
├── environments/ # Environment-specific configs
├── main.ts # Application bootstrap
└── styles.scss # Global styles

# Technology Stack

## Framework & Core Libraries

- **Angular**: v21.1.0 (standalone components architecture)
- **TypeScript**: v5.9.2 with strict mode enabled
- **RxJS**: v7.8.0 for reactive programming
- **Router**: Angular Router for navigation

## Build System

- **Angular CLI**: v21.1.2
- **Build Tool**: @angular/build (application builder)
- **Package Manager**: npm v11.8.0
- **Test Runner**: Vitest v4.0.8

## Styling

- **Preprocessor**: SCSS
- **Component Styles**: Inline SCSS with 4kB warning / 8kB error budget

## Code Quality

- **Prettier**: Configured with 100 character line width, single quotes
- **TypeScript**: Strict mode with additional compiler checks enabled
  - `noImplicitOverride`, `noPropertyAccessFromIndexSignature`
  - `noImplicitReturns`, `noFallthroughCasesInSwitch`
- **Angular Compiler**: Strict templates and injection parameters

Mong muốn:
chúng ta cần lựa chọn và tích hợp angular skills cho AI làm việc

code của app đang sai. hãy tổ chức lại nhé
Đầu tiên, các menu con của menu tài chính phải là menu cùng cấp với các menu như Bảng điều khiển, Quản lý tài khoản chứ không phải con của tài chính

Bỏ qua bước build để test.
Chỉ cần check lại code có lỗi ts hay không thôi.

Hãy sử dụng icon tương ứng với tính năng của các feature chứ?

Bây giờ hãy tổ chức lại router và structure code của hệ thống.
code trong src/app/features/finance đang bị tổ chức sai.
Hãy tổ chức lại đúng theo từng feature và routing của từng feature.
không cần giữ nguyên url là url con của /finance nhé

- Step 1: hãy phân tích api/app/modules/api/controllers xem api sẽ cần các quyền nào.
- Step 2: tạo group supper_admin với toàn bộ quyền trên
- Step 3: tạo tài khoản với email: trinhln.it@gmail.com mật khẩu: 123456 được gán quyền supper_admin để đăng nhập được vào hệ thống
