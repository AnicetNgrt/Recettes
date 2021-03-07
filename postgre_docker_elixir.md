# PostgreSQL + Docker + Elixir (avec Ecto)

*Auteur : Anicet Nougaret
Dernière maj : 06/02/2021*

## Prérequis
- Elixir + mix d'installé 
  - version : `Mix 1.11.2 (compiled with Erlang/OTP 21)`
  - `mix -v` pour vérifier
- Docker d'installé sur une machine linux (ou wsl)
  - version : `Docker version 20.10.2, build 2291f61`
  - `docker -v` pour vérifier

## Modificateurs
- Partout où `<project_name>` remplacer par le nom de votre projet en *snake_case*
- Partout où `<ProjectName>` remplacer par le nom de votre projet en *PascalCase*
- Partout où `<password>` remplacer par le mot de passe souhaité pour la base de donnée Postgresql
- Partout où `<public-adress>` remplacer par l'adresse IP publique de la machine qui héberge le conteneur Docker de postgres. `localhost` si c'est sur la même machine que le code elixir.

# Recette

## Préparer la partie Elixir
1. `mix new <project_name>` puis `cd <project_name>`
2. Dans `<project_name>/mix.exs`
    ```elixir
    defp deps do
      [
        {:postgrex, ">= 0.0.0"},
        {:ecto_sql, "~> 3.1"}
      ]
    end
    ```
3. `mix deps.get`
4. `mkdir config`
5. Dans `<project_name>/config/config.exs`
    ```elixir
    use Mix.Config

    config :<project_name>, :ecto_repos, [<ProjectName>.Repo]

    import_config "#{Mix.env}.exs"
    ```
6. Dans `<project_name>/config/dev.exs`
    ```elixir
    config :<project_name>, <ProjectName>.Repo, # https://hexdocs.pm/ecto_sql/Ecto.Adapters.Postgres.html#module-connection-options
      username: "postgres",
      password: "<password>",
      database: "<project_name>_dev",
      hostname: "<public-adress>",
      show_sensitive_data_on_connection_error: true
    ```
7. Dans `<project_name>/lib/<project_name>/repo.ex`
    ```elixir
    defmodule <ProjectName>.Repo do
      use Ecto.Repo,
          otp_app: :<project_name>,
          adapter: Ecto.Adapters.Postgres
    end
    ```
8. Dans `<project_name>/lib/<project_name>/application.ex`
    ```elixir
    defmodule <ProjectName>.Application do
      use Application

      @impl true
      def start(_type, _args) do
        children = [
          <ProjectName>.Repo
        ]

        opts = [strategy: :one_for_one, name: Linky.Supervisor]
        Supervisor.start_link(children, opts)
      end
    end
    ```
## Préparer Docker et Postgres
9. `docker pull postgres:alpine`
10. `docker run --name postgres-0 -e POSTGRES_PASSWORD=<password> -d -p 5432:5432 postgres:alpine`
11. `docker ps` pour vérifier
12. `psql -h <public-adress> -p 5432 -U postgres`
13. Entrez `<password>` si on vous le demande
14. `create table test;` puis `drop table test;` pour essayer
15. `exit;`

## Essayer le projet
16.  `mix ecto.create`
17. `iex -S mix`, s'il n'y a pas d'erreur, alors c'est que vous avez tout réussi !

## Pour aller plus loin ...
- [Apprendre à utiliser Ecto (en anglais)](https://www.youtube.com/playlist?list=PLFhQVxlaKQElscjMvMmyMCaZ9mxf4XAw-)