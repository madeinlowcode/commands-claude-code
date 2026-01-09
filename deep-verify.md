# Verificação Profunda Completa

## Name
deep-verify

## Trigger
`/deep-verify`

## Description
Executa uma verificação completa e profunda quando você identifica algo complexo.

## Usage
```bash
/deep-verify [tipo]
```

### Tipos Disponíveis
- `full` - Testes completos + tipos + lint + cobertura
- `test` - Apenas testes com relatório detalhado
- `types` - Verificação de tipos TypeScript
- `coverage` - Cobertura de testes

## Examples
```bash
/deep-verify
# Roda verificação completa por padrão

/deep-verify test
# Apenas testes com saída detalhada

/deep-verify coverage
# Mostra cobertura de testes
```

## Script
```bash
#!/bin/bash

GREEN='\\033[0;32m'
BLUE='\\033[0;34m'
RED='\\033[0;31m'
YELLOW='\\033[1;33m'
NC='\\033[0m'

TIPO=${1:-full}

echo -e "${BLUE}━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━${NC}"
echo -e "${BLUE}🔍 VERIFICAÇÃO: $TIPO${NC}"
echo -e "${BLUE}━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━${NC}\\n"

HAS_ERROR=0

run_check() {
  local name=$1
  local cmd=$2
  echo -e "${YELLOW}▶ $name...${NC}"
  if eval "$cmd"; then
    echo -e "${GREEN}✅ $name passou${NC}\\n"
  else
    echo -e "${RED}❌ $name falhou${NC}\\n"
    HAS_ERROR=1
  fi
}

case $TIPO in
  full)
    run_check "Testes" "npm test -- --passWithNoTests"
    run_check "TypeScript" "npm run type-check"
    run_check "Lint" "npm run lint"
    run_check "Cobertura" "npm test -- --coverage"
    ;;
  test)
    run_check "Testes Detalhados" "npm test -- --verbose --passWithNoTests"
    ;;
  types)
    run_check "Verificação de Tipos" "npm run type-check"
    ;;
  coverage)
    run_check "Cobertura de Testes" "npm test -- --coverage"
    ;;
  *)
    echo -e "${RED}Tipo desconhecido: $TIPO${NC}"
    exit 1
    ;;
esac

echo -e "${BLUE}━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━${NC}"
if [ $HAS_ERROR -eq 0 ]; then
  echo -e "${GREEN}✅ VERIFICAÇÕES PASSARAM!${NC}"
else
  echo -e "${RED}⚠️ VERIFICAÇÕES FALHARAM${NC}"
fi
echo -e "${BLUE}━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━${NC}\\n"

exit $HAS_ERROR
```
4. Faça commit: `git add .claude/ && git commit -m "feat: add deep-verify command"`

**Pronto!** Agora você pode usar `/deep-verify` no Claude Code quando precisar de verificações complexas. 🚀
