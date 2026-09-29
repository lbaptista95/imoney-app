# imoney-app

App do iMoney: React Native com Expo, consumido apenas pelo desenvolvedor no
próprio iPhone. Fala **somente** com o `imoney-api`, nunca direto com o
agregador, e não guarda segredo nenhum do agregador.

Este repositório é independente. Ele é desenvolvido a partir do workspace
`imoney-workspace`, que guarda as specs e clona este repo em `repos/app`.

## Estado

Ainda **sem código**. O esqueleto existe para que o workspace tenha o que clonar
e para que as guardas de commit estejam ativas desde o primeiro commit. O código
começa na Phase 2 do roadmap, depois de o contrato da API estabilizar.

## Stack decidida

Decidida nos ADRs do workspace, não aqui.

| Item | Escolha |
| --- | --- |
| SDK | Expo SDK 57 (React Native 0.86, React 19.2), fixado no MVP |
| Navegação | Expo Router |
| Dados | TanStack Query 5 + cliente gerado com `openapi-typescript` |
| Listas | FlashList 2 |
| Gráficos | react-native-gifted-charts (SVG, para o build web ser testável) |
| Auth | Entra External ID (OIDC + PKCE) + Face ID local |
| Distribuição | EAS Build, internal distribution ad hoc no iPhone |

## Convenções

- Commits em **Conventional Commits**, validados pelo hook `commit-msg`.
- Todo commit feito dentro de uma feature leva o trailer `Spec: <NNN-slug>`,
  apontando para `.specs/features/<NNN-slug>/` no workspace.
- O cliente da API é **gerado**, nunca escrito à mão. O CI regenera e falha se o
  resultado divergir do que está commitado.
- APIs nativas sem equivalente no web (`expo-secure-store`,
  `expo-local-authentication`) ficam atrás de um módulo com variante `.web.ts`,
  para o build web seguir testável com Playwright.
- `testID` em todo elemento interativo: no web ele vira `data-testid`, que é o
  seletor que o Playwright usa.
- Nada de dado financeiro real em fixture, teste ou log.

## Hooks

Instalados por `core.hooksPath` apontando para `.githooks/`, o que o
`ws.mjs sync` configura ao clonar. Se você clonar este repo fora do workspace, o
hook `commit-msg` não encontra o validador e recusa o commit por segurança.
