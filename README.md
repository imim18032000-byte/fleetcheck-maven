# FleetCheck – Build Systems Lab

This project is intentionally incomplete. Follow the worksheet in the order given.

Expected final application output:

```
FleetCheck 1.0
Vehicles loaded: 4
Vehicles requiring service: 2
Average mileage: 37000 km
```

Do not copy the solution POM. The objective is to observe how each build change alters the result.

## Evidence 1:

    [ERROR] /C:/Users/migue/Downloads/FleetCheck_Starter/FleetCheck_Starter/src/main/java/pt/upt/fleetcheck/App.java:[4,38] package com.fasterxml.jackson.databind does not exist

O erro é causado pelo import da linha 4 de App.java (com.fasterxml.jackson.databind.ObjectMapper), porque o pom.xml não declara a dependência jackson-databind.


## Evidence 4

Sem o Shade, o mvn clean package gera um JAR de ~6 KB que não executa:

    java -jar target\fleetcheck-1.0.0.jar
    no main manifest attribute, in target\fleetcheck-1.0.0.jar

Esse JAR só tem as classes do projeto, sem `Main-Class` no manifest e sem o Jackson.

Com o Shade, o build gera também o `fleetcheck-1.0.0-all.jar` (~2,3 MB), que corre:

    java -jar target\fleetcheck-1.0.0-all.jar
    FleetCheck 1.0
    Vehicles loaded: 4
    Vehicles requiring service: 2
    Average mileage: 37000 km

O Shade incluiu as dependências (Jackson) dentro do JAR e definiu `Main-Class: pt.upt.fleetcheck.App` no manifest, por isso ficou auto-contido e executável.

## Passo 5:
“O wrapper permite usar o Maven sem ser necessário ter a versão certa do Maven instalada.”

## Evidence 7 – SBOM (CycloneDX)

## Evidence 7

O SBOM contém o `jackson-core` e o `jackson-annotations` porque são dependências transitivas do `jackson-databind`. Eu só declarei o `jackson-databind` no `pom.xml`, mas o Maven resolve automaticamente tudo o que ele precisa, e o SBOM lista o grafo completo de dependências usadas pela aplicação.