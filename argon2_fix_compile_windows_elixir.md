# Argon2 Elixir : corriger l'erreur de compilation NMAKE
*Auteur : Anicet Nougaret
Dernière maj : 28/02/2021*

## Prérequis
- Un projet `mix` avec `argon2_elixir` en dépendence
- Visual studio 2019 d'installé sur windows

# Recette
- Dans le même terminal d'où vous compilez : `cmd /K "C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC\Auxiliary\Build\vcvarsall.bat" amd64` 