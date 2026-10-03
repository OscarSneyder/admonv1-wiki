# Matriz de trazabilidad

## Cadena PAREX
> ⚠️ **Por confirmar:** completar a medida que se formalicen C.U, RNF y mecanismos.

| SN | ESN | C.U | RNF | MEC | EUV | Evidencia |
|----|-----|-----|-----|-----|-----|-----------|
| SN-06 | ESN-01 | C.U-02 | — | MEC-01 | EUV-02 | ✏️ |
| SN-06 | ESN-01 | C.U-03 | RNF-01, RNF-03, RNF-05 | MEC-02, MEC-03 | EUV-03, EUV-04, EUV-05, EUV-Q-01, EUV-Q-03, EUV-Q-05 | k6 (EUV-Q-01) 🟡 |
| SN-06 | — | C.U-04 | RNF-02 | MEC-01 | EUV-06 | ✏️ |
| — | — | — | RNF-04 | MEC-04 | EUV-Q-04 | Pipeline ✅ |

## Trazabilidad técnica

"e2e" = `api-tests/test_api.py::test_create_application_end_to_end`, ejecutada en la etapa Test del pipeline.

Conecta cada requisito con su escenario, los componentes que lo implementan, el endpoint, la decisión arquitectónica y la prueba que lo verifica.

| RF / RNF | Escenario | Componentes front | Componentes back | Endpoint | ADR | Prueba |
|----------|-----------|-------------------|------------------|----------|-----|--------|
| RF-01 | EUV-01 | `LoginPage`, `Login`, `HttpAuthRepository`, `TokenStorage` | `AuthController`, `AuthService`, `JwtService` | `POST /api/auth/login` | 0003 | e2e (paso 1) |
| RF-02, RF-03 | EUV-02 | `ConvocationsPage`, `ConvocationDetailPage`, `HttpConvocationRepository` | `CallController`, `CallServiceImpl` | `GET /api/calls[/{id}]` | 0001 | e2e |
| RF-04 | EUV-03 | — (no invocado por el frontend) | `ApplicationController.check`, `ApplicationServiceImpl` | `GET /api/applications/check` | 0003 | e2e |
| RF-05, RF-06 | EUV-04 | `DocumentUpload`, `useDocumentUpload`, `UploadDocument`, `HttpDocumentRepository` | `DocumentController`, `DocumentServiceImpl`, `S3Config` | `/api/documents/**` | 0004 | e2e |
| RF-07 | EUV-05 | `CreateApplicationPage`, `CreateApplication`, `HttpApplicationRepository` | `ApplicationController`, `ApplicationServiceImpl` | `POST /api/applications` | 0001, 0005 | e2e (paso 7), k6 (carga) |
| RF-08, RF-09 | EUV-06 | `ApplicationsPage`, `ApplicationDetailPage` | `ApplicationController`, `ApplicationSpecification` | `GET /api/applications[/{id}]` | 0001 | e2e |
| RNF-01 | EUV-Q-01 | — | Backend completo | `POST /api/applications` | 0004, 0005 | k6 300 VU 🟡 |
| RNF-02 | EUV-Q-02 | `ProtectedRoute`, `authenticatedFetch` | `SecurityConfig`, `JwtConfig` | todos | 0003 | ✏️ |
| RNF-03 | EUV-Q-03 | — | `ApplicationServiceImpl`, restricción única | `POST /api/applications` | 0005 | ✏️ |
| RNF-04 | EUV-Q-04 | — | `buildspec*.yml`, `appspec.yml`, `deploy/` | `/api/health` | 0006 | pipeline ✅ |
| RNF-05 | EUV-Q-05 | `useDocumentUpload` | `DocumentServiceImpl` | `/api/documents/**` | 0004 | ✏️ |

> Para una versión filtrable, exportar a `assets/documentos/trazabilidad.xlsx`.
