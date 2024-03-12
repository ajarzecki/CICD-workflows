# CICD-workflows

## Założenia

### Secrets
CLASPRC_LOGIN, jego zawartością ma być zakodowana w BASE64 zawartość pliku .clasprc.json po wydaniu komendy clasp login i zalogogowaniu się na konto, z którego ma byc udostępniany scrypt GAS
CLASPRC_RUN: TODO, związane z uruchamianiem testów

### Variables
SCRIPT_ID - wskazująca na produkcyjny ID danego skryptu 
PROJECT_ID - TODO, związane z uruchamianiem testów
TEST_METHOD - nazwa metody do uruchomienia testów


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
        uses: ajarzecki/CICD-workflows/.github/workflows/deploy.yamlmain
        with:
          SCRIPT_ID: ${{ vars.SCRIPT_ID }}
          PROJECT_ID: ${{ vars.PROJECT_ID }}
          TEST_METHOD: ${{ vars.TEST_METHOD }}
        secrets:
          CLASPRC_LOGIN: "${{ secrets.CLASPRC_LOGIN }}"
          CLASPRC_RUN: "${{ secrets.CLASPRC_RUN }}"

## Użycie
W celu wydania nowej wersji należy zrobić nowego TAGa i zrobić nowy release na podstawie tego TAGa o nazwie release_DOWOLNU_OPIS

Wywołuje on zdefiniowany workflow, który robi deploy nowej wersji aktualnego repo w ramach GAS, dla wartości SCRIPT_ID zdefiniowanej w zmiennych repo w ramach konta 


## Przygotowanie do uruchamiania testów

https://console.cloud.google.com/

Create New project

	
Enable APIs and services -> "Apps Script API"

Credential -> Configure Consent screen
Credential -> OAuth 2.0 -> ClientID -> desktop
Download json

https://console.cloud.google.com/iam-admin/settings?project=woocomerce-bridge-preprod
Setting -> get Project number

https://script.google.com/home/projects/1KZERZMmzRoQU48I1cn1f7LofBKqDhd-tlt69GzXtmcXHuQ8gauwX3kMe/edit
Settings -> set GCP project number 

Add to appscript.json"

executionApi": {
    "access": "ANYONE"
  }
  
 pull SCRIPT_ID
 
 clasp login --creds ~/creds.json
 
 add project ID to  .clasp.json
 
 {"scriptId":"ID_SCRYPTU","rootDir":"/root/dev","projectId":"ID_PROJEKTU"}
