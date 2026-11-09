# Locadora de Veículos

## Funcionalidades

- Cadastro de **veículos** (placa validada, categoria e diária)
- Cadastro de **clientes** (CPF validado e sem duplicidade)
- Registro de **locações** com cálculo automático do valor
- **Devolução** com cálculo de multa por atraso
- Painel com resumo da frota
- Tema claro/escuro automático e layout responsivo

## Regras de negócio

| Categoria  | Diária     |
|------------|-----------:|
| Econômico  | R$ 90,00   |
| Sedan      | R$ 150,00  |
| SUV        | R$ 220,00  |
| Luxo       | R$ 400,00  |

- **Total** = diária × dias contratados.
- **Atraso**: cada dia excedente custa **120% da diária**.
- Devolver antes do prazo **não** gera desconto.
- Um veículo alugado não pode ser alugado novamente até a devolução.
- Período de locação: de 1 a 90 dias.

## Como executar

Requisitos: **JDK 21+**.

```bash
./scripts/run.sh          # abre em http://localhost:8080
./scripts/run.sh 9090     # porta personalizada
./scripts/test.sh         # roda os testes
```

No Windows, sem bash:

```bat
mkdir out
javac -encoding UTF-8 -d out src\main\java\com\locadora\*.java src\main\java\com\locadora\*\*.java
xcopy /E /I /Y src\main\resources out
java -cp out com.locadora.Main
```

Com Maven (opcional): `mvn package` gera `target/locadora-veiculos-1.0.0.jar`.

## Estrutura

```
locadora-veiculos/
├── pom.xml                      # opcional (Maven)
├── scripts/                     # build.sh, run.sh, test.sh
└── src/
    ├── main/
    │   ├── java/com/locadora/
    │   │   ├── Main.java
    │   │   ├── model/           # Veiculo, Cliente, Locacao, Categoria
    │   │   ├── repository/      # persistência em memória
    │   │   ├── service/         # regras de negócio (LocadoraService)
    │   │   └── web/             # Servidor (rotas), Paginas, Html
    │   └── resources/
    │       ├── templates/       # layout, home, veiculos, clientes, locacoes (.html)
    │       └── static/style.css
    └── test/java/com/locadora/  # LocadoraServiceTeste
```

## Rotas

| Método | Rota                 | Descrição                  |
|--------|----------------------|----------------------------|
| GET    | `/`                  | Painel                     |
| GET    | `/veiculos`          | Lista e formulário         |
| POST   | `/veiculos`          | Cadastra veículo           |
| GET    | `/clientes`          | Lista e formulário         |
| POST   | `/clientes`          | Cadastra cliente           |
| GET    | `/locacoes`          | Lista e formulário         |
| POST   | `/locacoes`          | Registra locação           |
| POST   | `/locacoes/devolver` | Registra devolução         |

## Observações

- Os dados ficam **em memória**: reiniciar o servidor volta aos dados de demonstração (`DadosIniciais`).
- Toda saída dinâmica passa por `Html.esc`, evitando XSS.
- A camada `repository` é isolada: trocar por JDBC/JPA (ex.: PostgreSQL) exige mudar só essas classes.

## Ideias de evolução

- Persistência com banco de dados (JDBC ou Spring Data JPA)
- Autenticação de funcionários
- Reserva por datas futuras e histórico por cliente
- Migrar a camada web para Spring Boot + Thymeleaf (os templates já estão em HTML)

## Licença

MIT
