# CICD-workflows

## Założenia

### Secrets
CLASPRC_JSON, jego zawartością ma być zakodowana w BASE64 zawartość pliku .clasprc.json po wydaniu komendy clasp login i zalogogowaniu się na konto scripts@dlarodziny.eu


W repo zdefiniowana zmienna SCRIPT_ID wskazująca na produkcyjny ID danego skryptu (np. rekolekcje-core-FdR )

## workflow
Zdefiniowanie w danym repo workflow o następującej zawartości

    name: Deploy-GAS
    run-name: ${{ github.actor }} deploy GAS
    on: 
        create:
            tags:
                release*
    jobs:
      job1:
        \# _VERSION_ konkretny commit 
        uses: DlaRodziny/CICD-workflows/.github/workflows/deploy.yaml@_VERSION_
        with:
          SCRIPT_ID: ${{ vars.SCRIPT_ID }}
        secrets:
          CLASPRC_JSON: "${{ secrets.CLASPRC_JSON }}"

## Użycie
W celu wydania nowej wersji należy zrobić nowego TAGa i zrobić nowy release na podstawie tego TAGa o nazwie release_DOWOLNU_OPIS

Wywołuje on zdefiniowany workflow, który robi deploy nowej wersji aktualnego repo w ramach GAS, dla wartości SCRIPT_ID zdefiniowanej w zmiennych repo w ramach konta scipts@dlarodziny.eu