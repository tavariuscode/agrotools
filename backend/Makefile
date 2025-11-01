.PHONY: help

# Cores para output
BLUE := \033[0;34m
GREEN := \033[0;32m
YELLOW := \033[0;33m
RED := \033[0;31m
NC := \033[0m # No Color

##@ 🎯 Ajuda

help: ## Mostra esta mensagem de ajuda
	@echo "$(BLUE)━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━$(NC)"
	@echo "$(GREEN)  🚀 Laravel Docker - Comandos Disponíveis$(NC)"
	@echo "$(BLUE)━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━$(NC)"
	@awk 'BEGIN {FS = ":.*##"; printf "\n"} /^[a-zA-Z_-]+:.*?##/ { printf "  $(YELLOW)%-25s$(NC) %s\n", $$1, $$2 } /^##@/ { printf "\n$(BLUE)%s$(NC)\n", substr($$0, 5) } ' $(MAKEFILE_LIST)
	@echo ""

##@ 🐳 Docker Compose

up: ## Inicia os containers em background
	@echo "$(GREEN)🚀 Iniciando containers...$(NC)"
	docker compose up -d

up-build: ## Rebuild e inicia os containers
	@echo "$(GREEN)🔨 Rebuilding e iniciando containers...$(NC)"
	docker compose up -d --build

down: ## Para e remove os containers
	@echo "$(RED)🛑 Parando containers...$(NC)"
	docker compose down

down-volumes: ## Para containers e remove volumes (⚠️ APAGA DADOS!)
	@echo "$(RED)⚠️  ATENÇÃO: Isso vai apagar todos os dados!$(NC)"
	@read -p "Tem certeza? [s/N]: " confirm; \
	if [ "$$confirm" = "s" ] || [ "$$confirm" = "S" ]; then \
		docker compose down -v; \
	else \
		echo "Cancelado."; \
	fi

restart: ## Reinicia todos os containers
	@echo "$(YELLOW)🔄 Reiniciando containers...$(NC)"
	docker compose restart

restart-php: ## Reinicia apenas o container PHP
	@echo "$(YELLOW)🔄 Reiniciando PHP...$(NC)"
	docker compose restart php

restart-nginx: ## Reinicia apenas o container Nginx
	@echo "$(YELLOW)🔄 Reiniciando Nginx...$(NC)"
	docker compose restart nginx

restart-mysql: ## Reinicia apenas o container MySQL
	@echo "$(YELLOW)🔄 Reiniciando MySQL...$(NC)"
	docker compose restart mysql

restart-postgres: ## Reinicia apenas o container PostgreSQL
	@echo "$(YELLOW)🔄 Reiniciando PostgreSQL...$(NC)"
	docker compose restart postgres

restart-mongodb: ## Reinicia apenas o container MongoDB
	@echo "$(YELLOW)🔄 Reiniciando MongoDB...$(NC)"
	docker compose restart mongodb

restart-redis: ## Reinicia apenas o container Redis
	@echo "$(YELLOW)🔄 Reiniciando Redis...$(NC)"
	docker compose restart redis

ps: ## Lista status dos containers
	docker compose ps

logs: ## Mostra logs de todos os containers
	docker compose logs -f

logs-php: ## Mostra logs do container PHP
	docker compose logs -f php

logs-nginx: ## Mostra logs do container Nginx
	docker compose logs -f nginx

logs-mysql: ## Mostra logs do container MySQL
	docker compose logs -f mysql

logs-postgres: ## Mostra logs do container PostgreSQL
	docker compose logs -f postgres

logs-mongodb: ## Mostra logs do container MongoDB
	docker compose logs -f mongodb

logs-redis: ## Mostra logs do container Redis
	docker compose logs -f redis

##@ 💻 Acesso aos Containers

bash: ## Acessa bash do container PHP
	docker compose exec php bash

bash-nginx: ## Acessa bash do container Nginx
	docker compose exec nginx sh

bash-mysql: ## Acessa bash do container MySQL
	docker compose exec mysql bash

bash-postgres: ## Acessa bash do container PostgreSQL
	docker compose exec postgres sh

bash-mongodb: ## Acessa bash do container MongoDB
	docker compose exec mongodb bash

bash-redis: ## Acessa bash do container Redis
	docker compose exec redis sh

db: ## Conecta ao MySQL (root)
	docker compose exec mysql mysql -u root -proot

db-dev: ## Conecta ao MySQL (developer)
	docker compose exec mysql mysql -u developer -p123456 db_laravel

psql: ## Conecta ao PostgreSQL (developer)
	docker compose exec postgres psql -U developer -d db_laravel

psql-root: ## Conecta ao PostgreSQL (postgres)
	docker compose exec postgres psql -U postgres

mongo: ## Conecta ao MongoDB (developer)
	docker compose exec mongodb mongosh -u developer -p 123456 --authenticationDatabase admin db_laravel

mongo-root: ## Conecta ao MongoDB (root)
	docker compose exec mongodb mongosh -u root -p 123456 --authenticationDatabase admin

mongo-shell: ## Abre MongoDB shell interativo
	docker compose exec mongodb mongosh

redis-cli: ## Acessa Redis CLI
	docker compose exec redis redis-cli

##@ 🎬 Setup & Instalação

setup: ## Setup inicial completo do projeto
	@echo "$(GREEN)🎬 Configurando projeto Laravel...$(NC)"
	@echo "$(YELLOW)📋 1/5 Copiando .env...$(NC)"
	cp --update=none laravel/.env.example laravel/.env
	@echo "$(YELLOW)📦 2/5 Instalando dependências Composer...$(NC)"
	docker compose exec php composer install
	@echo "$(YELLOW)🔑 3/5 Gerando chave da aplicação...$(NC)"
	docker compose exec php php artisan key:generate
	@echo "$(YELLOW)🗄️  4/5 Executando migrations...$(NC)"
	docker compose exec php php artisan migrate
	@echo "$(YELLOW)🔗 5/5 Criando link de storage...$(NC)"
	docker compose exec php php artisan storage:link
	@echo "$(GREEN)✅ Setup concluído!$(NC)"

setup-full: ## Setup completo com seed
	@echo "$(GREEN)🎬 Setup completo com seed...$(NC)"
	$(MAKE) setup
	@echo "$(YELLOW)🌱 Executando seeders...$(NC)"
	docker compose exec php php artisan db:seed
	@echo "$(GREEN)✅ Setup completo concluído!$(NC)"

install: ## Apenas instala dependências Composer
	@echo "$(YELLOW)📦 Instalando dependências...$(NC)"
	docker compose exec php composer install

install-dev: ## Instala dependências de desenvolvimento
	@echo "$(YELLOW)📦 Instalando dependências de dev...$(NC)"
	docker compose exec php composer install --dev

update: ## Atualiza dependências Composer
	@echo "$(YELLOW)⬆️  Atualizando dependências...$(NC)"
	docker compose exec php composer update

##@ 🗄️ Banco de Dados - Migrations

migrate: ## Executa migrations pendentes
	@echo "$(GREEN)🗄️  Executando migrations...$(NC)"
	docker compose exec php php artisan migrate

migrate-fresh: ## Dropa tudo e recria (⚠️ APAGA DADOS!)
	@echo "$(RED)⚠️  ATENÇÃO: Isso vai apagar todos os dados!$(NC)"
	@read -p "Tem certeza? [s/N]: " confirm; \
	if [ "$$confirm" = "s" ] || [ "$$confirm" = "S" ]; then \
		docker compose exec php php artisan migrate:fresh; \
	else \
		echo "Cancelado."; \
	fi

migrate-fresh-seed: ## Dropa, recria e popula o banco
	@echo "$(RED)⚠️  ATENÇÃO: Isso vai apagar todos os dados!$(NC)"
	@read -p "Tem certeza? [s/N]: " confirm; \
	if [ "$$confirm" = "s" ] || [ "$$confirm" = "S" ]; then \
		docker compose exec php php artisan migrate:fresh --seed; \
	else \
		echo "Cancelado."; \
	fi

migrate-refresh: ## Rollback e re-executa todas migrations
	docker compose exec php php artisan migrate:refresh

migrate-refresh-seed: ## Rollback, re-executa e popula
	docker compose exec php php artisan migrate:refresh --seed

migrate-rollback: ## Desfaz última migração
	docker compose exec php php artisan migrate:rollback

migrate-rollback-step: ## Desfaz N migrações
	@read -p "Quantos steps desfazer? " steps; \
	docker compose exec php php artisan migrate:rollback --step=$$steps

migrate-reset: ## Desfaz todas as migrações
	docker compose exec php php artisan migrate:reset

migrate-status: ## Mostra status das migrations
	docker compose exec php php artisan migrate:status

migrate-install: ## Instala repositório de migrations
	docker compose exec php php artisan migrate:install

schema-dump: ## Dump do schema do banco de dados
	docker compose exec php php artisan schema:dump

##@ 🗄️ Banco de Dados - Operações

db-show: ## Mostra informações do banco
	docker compose exec php php artisan db:show

db-table: ## Mostra estrutura de uma tabela
	@read -p "Nome da tabela: " table; \
	docker compose exec php php artisan db:table $$table

db-monitor: ## Monitora conexões do banco
	docker compose exec php php artisan db:monitor

db-wipe: ## Limpa todas as tabelas (⚠️ APAGA DADOS!)
	@echo "$(RED)⚠️  ATENÇÃO: Isso vai apagar todos os dados!$(NC)"
	@read -p "Tem certeza? [s/N]: " confirm; \
	if [ "$$confirm" = "s" ] || [ "$$confirm" = "S" ]; then \
		docker compose exec php php artisan db:wipe; \
	else \
		echo "Cancelado."; \
	fi

seed: ## Executa todos os seeders
	@echo "$(GREEN)🌱 Executando seeders...$(NC)"
	docker compose exec php php artisan db:seed

seed-class: ## Executa um seeder específico
	@read -p "Nome do seeder: " seeder; \
	docker compose exec php php artisan db:seed --class=$$seeder

##@ 💾 Backup & Restore

backup-db: ## Faz backup do banco de dados
	@echo "$(GREEN)💾 Criando backup do banco...$(NC)"
	@mkdir -p backups
	@timestamp=$$(date +%Y%m%d_%H%M%S); \
	docker compose exec mysql mysqldump -u root -proot db_laravel > backups/db_backup_$$timestamp.sql && \
	echo "$(GREEN)✅ Backup salvo em: backups/db_backup_$$timestamp.sql$(NC)"

restore-db: ## Restaura backup do banco
	@echo "$(YELLOW)Arquivos disponíveis em backups/:$(NC)"
	@ls -lh backups/*.sql 2>/dev/null || echo "Nenhum backup encontrado"
	@read -p "Nome do arquivo (sem o path): " file; \
	if [ -f "backups/$$file" ]; then \
		echo "$(YELLOW)Restaurando backup...$(NC)"; \
		docker compose exec -T mysql mysql -u root -proot db_laravel < backups/$$file && \
		echo "$(GREEN)✅ Backup restaurado!$(NC)"; \
	else \
		echo "$(RED)❌ Arquivo não encontrado!$(NC)"; \
	fi

backup-postgres: ## Faz backup do PostgreSQL
	@echo "$(GREEN)💾 Criando backup do PostgreSQL...$(NC)"
	@mkdir -p backups
	@timestamp=$$(date +%Y%m%d_%H%M%S); \
	docker compose exec postgres pg_dump -U developer db_laravel > backups/postgres_backup_$$timestamp.sql && \
	echo "$(GREEN)✅ Backup salvo em: backups/postgres_backup_$$timestamp.sql$(NC)"

backup-postgres-custom: ## Faz backup customizado do PostgreSQL (com dados e estrutura)
	@echo "$(GREEN)💾 Criando backup customizado do PostgreSQL...$(NC)"
	@mkdir -p backups
	@timestamp=$$(date +%Y%m%d_%H%M%S); \
	docker compose exec postgres pg_dump -U developer -Fc db_laravel > backups/postgres_custom_$$timestamp.dump && \
	echo "$(GREEN)✅ Backup customizado salvo em: backups/postgres_custom_$$timestamp.dump$(NC)"

restore-postgres: ## Restaura backup do PostgreSQL
	@echo "$(YELLOW)Arquivos disponíveis em backups/:$(NC)"
	@ls -lh backups/postgres_*.sql backups/postgres_*.dump 2>/dev/null || echo "Nenhum backup encontrado"
	@read -p "Nome do arquivo (sem o path): " file; \
	if [ -f "backups/$$file" ]; then \
		echo "$(YELLOW)Restaurando backup...$(NC)"; \
		if echo "$$file" | grep -q "\.dump$$"; then \
			docker compose exec -T postgres pg_restore -U developer -d db_laravel < backups/$$file && \
			echo "$(GREEN)✅ Backup customizado restaurado!$(NC)"; \
		else \
			docker compose exec -T postgres psql -U developer -d db_laravel < backups/$$file && \
			echo "$(GREEN)✅ Backup SQL restaurado!$(NC)"; \
		fi \
	else \
		echo "$(RED)❌ Arquivo não encontrado!$(NC)"; \
	fi

psql-list-databases: ## Lista todos os bancos de dados PostgreSQL
	docker compose exec postgres psql -U developer -l

psql-list-tables: ## Lista todas as tabelas do banco
	docker compose exec postgres psql -U developer -d db_laravel -c "\dt"

psql-describe-table: ## Descreve estrutura de uma tabela
	@read -p "Nome da tabela: " table; \
	docker compose exec postgres psql -U developer -d db_laravel -c "\d $$table"

psql-exec: ## Executa comando SQL no PostgreSQL
	@read -p "Comando SQL: " query; \
	docker compose exec postgres psql -U developer -d db_laravel -c "$$query"

psql-vacuum: ## Executa VACUUM no banco (limpeza e otimização)
	@echo "$(YELLOW)🔄 Executando VACUUM...$(NC)"
	docker compose exec postgres psql -U developer -d db_laravel -c "VACUUM FULL ANALYZE;"
	@echo "$(GREEN)✅ VACUUM concluído!$(NC)"

psql-stats: ## Mostra estatísticas do PostgreSQL
	docker compose exec postgres psql -U developer -d db_laravel -c "SELECT pg_size_pretty(pg_database_size('db_laravel')) AS database_size;"

psql-connections: ## Mostra conexões ativas
	docker compose exec postgres psql -U developer -d db_laravel -c "SELECT pid, usename, application_name, client_addr, state FROM pg_stat_activity;"

##@ 🧹 Cache & Otimização

cache-clear: ## Limpa cache da aplicação
	docker compose exec php php artisan cache:clear

cache-forget: ## Remove item específico do cache
	@read -p "Chave do cache: " key; \
	docker compose exec php php artisan cache:forget $$key

cache-prune-tags: ## Remove tags stale do cache (Redis)
	docker compose exec php php artisan cache:prune-stale-tags

config-clear: ## Limpa cache de configuração
	docker compose exec php php artisan config:clear

config-publish: ## Publica arquivos de configuração
	docker compose exec php php artisan config:publish

route-clear: ## Limpa cache de rotas
	docker compose exec php php artisan route:clear

view-clear: ## Limpa cache de views
	docker compose exec php php artisan view:clear

event-clear: ## Limpa cache de eventos
	docker compose exec php php artisan event:clear

event-list: ## Lista eventos e listeners
	docker compose exec php php artisan event:list

clear-compiled: ## Remove arquivo de classe compilada
	docker compose exec php php artisan clear-compiled

clear-all: ## Limpa TODOS os caches
	@echo "$(GREEN)🧹 Limpando todos os caches...$(NC)"
	docker compose exec php php artisan optimize:clear
	docker compose exec php php artisan cache:clear
	docker compose exec php php artisan config:clear
	docker compose exec php php artisan route:clear
	docker compose exec php php artisan view:clear
	docker compose exec php php artisan event:clear
	@echo "$(GREEN)✅ Todos os caches limpos!$(NC)"

optimize: ## Otimiza aplicação (produção)
	@echo "$(GREEN)⚡ Otimizando aplicação...$(NC)"
	docker compose exec php php artisan optimize

optimize-clear: ## Remove todas otimizações
	docker compose exec php php artisan optimize:clear

config-cache: ## Cria cache de configuração
	docker compose exec php php artisan config:cache

route-cache: ## Cria cache de rotas
	docker compose exec php php artisan route:cache

event-cache: ## Cria cache de eventos
	docker compose exec php php artisan event:cache

production-ready: ## Prepara app para produção
	@echo "$(GREEN)🚀 Preparando para produção...$(NC)"
	docker compose exec php php artisan config:cache
	docker compose exec php php artisan route:cache
	docker compose exec php php artisan view:cache
	docker compose exec php php artisan event:cache
	docker compose exec php php artisan optimize
	@echo "$(GREEN)✅ Aplicação otimizada para produção!$(NC)"

##@ 🔑 Chaves & Segurança

key-generate: ## Gera nova chave da aplicação
	docker compose exec php php artisan key:generate

storage-link: ## Cria link simbólico de storage
	docker compose exec php php artisan storage:link

storage-unlink: ## Remove link simbólico de storage
	docker compose exec php php artisan storage:unlink

permissions: ## Corrige permissões do Laravel
	@echo "$(YELLOW)🔐 Corrigindo permissões...$(NC)"
	docker compose exec php chmod -R 775 storage bootstrap/cache
	docker compose exec php chown -R www-data:www-data storage bootstrap/cache
	@echo "$(GREEN)✅ Permissões corrigidas!$(NC)"

##@ 📋 Desenvolvimento

tinker: ## Abre Laravel Tinker (REPL)
	docker compose exec php php artisan tinker

route-list: ## Lista todas as rotas
	docker compose exec php php artisan route:list

route-list-full: ## Lista rotas com detalhes completos
	docker compose exec php php artisan route:list --columns=uri,name,action,middleware

about: ## Mostra informações sobre a aplicação
	docker compose exec php php artisan about

inspire: ## Mensagem inspiradora
	docker compose exec php php artisan inspire

docs: ## Abre documentação do Laravel
	docker compose exec php php artisan docs

channel-list: ## Lista canais de broadcast
	docker compose exec php php artisan channel:list

lang-publish: ## Publica arquivos de idioma
	docker compose exec php php artisan lang:publish

auth-clear-resets: ## Limpa tokens de reset de senha expirados
	docker compose exec php php artisan auth:clear-resets

install-api: ## Instala estrutura de API (Sanctum/Passport)
	docker compose exec php php artisan install:api

serve: ## Servidor de desenvolvimento PHP
	docker compose exec php php artisan serve --host=0.0.0.0 --port=8000

##@ 🧪 Testes

test: ## Executa todos os testes
	@echo "$(GREEN)🧪 Executando testes...$(NC)"
	docker compose exec php php artisan test

test-coverage: ## Executa testes com coverage
	docker compose exec php php artisan test --coverage

test-parallel: ## Executa testes em paralelo
	docker compose exec php php artisan test --parallel

test-filter: ## Executa teste específico
	@read -p "Filtro de teste: " filter; \
	docker compose exec php php artisan test --filter=$$filter

pest: ## Executa Pest tests
	docker compose exec php ./vendor/bin/pest

phpunit: ## Executa PHPUnit
	docker compose exec php ./vendor/bin/phpunit

##@ 📦 Composer

composer-install: ## Instala dependências
	docker compose exec php composer install

composer-update: ## Atualiza dependências
	docker compose exec php composer update

composer-require: ## Adiciona novo pacote
	@read -p "Nome do pacote: " package; \
	docker compose exec php composer require $$package

composer-require-dev: ## Adiciona pacote de dev
	@read -p "Nome do pacote: " package; \
	docker compose exec php composer require --dev $$package

composer-remove: ## Remove pacote
	@read -p "Nome do pacote: " package; \
	docker compose exec php composer remove $$package

composer-dump: ## Regenera autoload
	docker compose exec php composer dump-autoload

composer-validate: ## Valida composer.json
	docker compose exec php composer validate

composer-outdated: ## Mostra pacotes desatualizados
	docker compose exec php composer outdated

composer-show: ## Mostra todos os pacotes instalados
	docker compose exec php composer show

##@ 📦 NPM / Node (se houver frontend)

npm-install: ## Instala dependências NPM
	docker compose exec php npm install

npm-dev: ## Build de desenvolvimento
	docker compose exec php npm run dev

npm-watch: ## Watch de desenvolvimento
	docker compose exec php npm run watch

npm-build: ## Build de produção
	docker compose exec php npm run build

npm-update: ## Atualiza dependências NPM
	docker compose exec php npm update

npm-outdated: ## Mostra pacotes NPM desatualizados
	docker compose exec php npm outdated

##@ 🔄 Queue (Filas)

queue-work: ## Processa filas
	docker compose exec php php artisan queue:work

queue-work-daemon: ## Processa filas em background
	docker compose exec -d php php artisan queue:work

queue-listen: ## Escuta filas
	docker compose exec php php artisan queue:listen

queue-restart: ## Reinicia workers
	docker compose exec php php artisan queue:restart

queue-retry: ## Reprocessa jobs falhados
	docker compose exec php php artisan queue:retry all

queue-retry-id: ## Reprocessa job específico
	@read -p "ID do job: " id; \
	docker compose exec php php artisan queue:retry $$id

queue-retry-batch: ## Reprocessa batch de jobs
	@read -p "ID do batch: " id; \
	docker compose exec php php artisan queue:retry-batch $$id

queue-forget: ## Remove job falhado
	@read -p "ID do job: " id; \
	docker compose exec php php artisan queue:forget $$id

queue-clear: ## Limpa jobs de uma fila
	@read -p "Nome da fila (ou deixe vazio para default): " queue; \
	if [ -z "$$queue" ]; then \
		docker compose exec php php artisan queue:clear; \
	else \
		docker compose exec php php artisan queue:clear $$queue; \
	fi

queue-flush: ## Limpa jobs falhados
	docker compose exec php php artisan queue:flush

queue-prune-failed: ## Remove jobs falhados antigos
	@read -p "Horas (padrão: 24): " hours; \
	docker compose exec php php artisan queue:prune-failed --hours=$${hours:-24}

queue-prune-batches: ## Remove batches antigos
	@read -p "Horas (padrão: 24): " hours; \
	docker compose exec php php artisan queue:prune-batches --hours=$${hours:-24}

queue-failed: ## Lista jobs falhados
	docker compose exec php php artisan queue:failed

queue-monitor: ## Monitora filas
	docker compose exec php php artisan queue:monitor

queue-table: ## Cria tabela de jobs
	docker compose exec php php artisan queue:table
	docker compose exec php php artisan migrate

queue-failed-table: ## Cria tabela de jobs falhados
	docker compose exec php php artisan queue:failed-table
	docker compose exec php php artisan migrate

queue-batches-table: ## Cria tabela de batches
	docker compose exec php php artisan queue:batches-table
	docker compose exec php php artisan migrate

##@ ⏰ Schedule (Agendamentos)

schedule-run: ## Executa schedule uma vez
	docker compose exec php php artisan schedule:run

schedule-work: ## Executa schedule continuamente
	docker compose exec php php artisan schedule:work

schedule-list: ## Lista comandos agendados
	docker compose exec php php artisan schedule:list

schedule-test: ## Testa schedule
	@read -p "Comando ou classe: " command; \
	docker compose exec php php artisan schedule:test $$command

schedule-clear-cache: ## Limpa cache do scheduler
	docker compose exec php php artisan schedule:clear-cache

schedule-interrupt: ## Interrompe execução atual do scheduler
	docker compose exec php php artisan schedule:interrupt

##@ 🛠️ Criação de Arquivos (Make)

make-controller: ## Cria controller
	@read -p "Nome do controller: " name; \
	docker compose exec php php artisan make:controller $$name

make-controller-resource: ## Cria resource controller
	@read -p "Nome do controller: " name; \
	docker compose exec php php artisan make:controller $$name --resource

make-controller-api: ## Cria API controller
	@read -p "Nome do controller: " name; \
	docker compose exec php php artisan make:controller $$name --api

make-model: ## Cria model
	@read -p "Nome do model: " name; \
	docker compose exec php php artisan make:model $$name

make-model-full: ## Cria model completo (migration, factory, seeder, controller)
	@read -p "Nome do model: " name; \
	docker compose exec php php artisan make:model $$name -mfsc

make-model-all: ## Cria model com tudo
	@read -p "Nome do model: " name; \
	docker compose exec php php artisan make:model $$name --all

make-migration: ## Cria migration
	@read -p "Nome da migration: " name; \
	docker compose exec php php artisan make:migration $$name

make-seeder: ## Cria seeder
	@read -p "Nome do seeder: " name; \
	docker compose exec php php artisan make:seeder $$name

make-factory: ## Cria factory
	@read -p "Nome da factory: " name; \
	docker compose exec php php artisan make:factory $$name

make-middleware: ## Cria middleware
	@read -p "Nome do middleware: " name; \
	docker compose exec php php artisan make:middleware $$name

make-request: ## Cria form request
	@read -p "Nome do request: " name; \
	docker compose exec php php artisan make:request $$name

make-resource: ## Cria API resource
	@read -p "Nome do resource: " name; \
	docker compose exec php php artisan make:resource $$name

make-policy: ## Cria policy
	@read -p "Nome da policy: " name; \
	docker compose exec php php artisan make:policy $$name

make-rule: ## Cria validation rule
	@read -p "Nome da rule: " name; \
	docker compose exec php php artisan make:rule $$name

make-job: ## Cria job
	@read -p "Nome do job: " name; \
	docker compose exec php php artisan make:job $$name

make-event: ## Cria event
	@read -p "Nome do event: " name; \
	docker compose exec php php artisan make:event $$name

make-listener: ## Cria listener
	@read -p "Nome do listener: " name; \
	docker compose exec php php artisan make:listener $$name

make-mail: ## Cria mailable
	@read -p "Nome do mail: " name; \
	docker compose exec php php artisan make:mail $$name

make-notification: ## Cria notification
	@read -p "Nome da notification: " name; \
	docker compose exec php php artisan make:notification $$name

make-observer: ## Cria observer
	@read -p "Nome do observer: " name; \
	docker compose exec php php artisan make:observer $$name

make-provider: ## Cria service provider
	@read -p "Nome do provider: " name; \
	docker compose exec php php artisan make:provider $$name

make-command: ## Cria artisan command
	@read -p "Nome do command: " name; \
	docker compose exec php php artisan make:command $$name

make-test: ## Cria test
	@read -p "Nome do test: " name; \
	docker compose exec php php artisan make:test $$name

make-test-unit: ## Cria unit test
	@read -p "Nome do test: " name; \
	docker compose exec php php artisan make:test $$name --unit

make-component: ## Cria Blade component
	@read -p "Nome do component: " name; \
	docker compose exec php php artisan make:component $$name

make-cast: ## Cria custom cast
	@read -p "Nome do cast: " name; \
	docker compose exec php php artisan make:cast $$name

make-channel: ## Cria broadcast channel
	@read -p "Nome do channel: " name; \
	docker compose exec php php artisan make:channel $$name

make-exception: ## Cria exception
	@read -p "Nome da exception: " name; \
	docker compose exec php php artisan make:exception $$name

make-class: ## Cria classe genérica
	@read -p "Nome da classe: " name; \
	docker compose exec php php artisan make:class $$name

make-enum: ## Cria enum
	@read -p "Nome do enum: " name; \
	docker compose exec php php artisan make:enum $$name

make-interface: ## Cria interface
	@read -p "Nome da interface: " name; \
	docker compose exec php php artisan make:interface $$name

make-trait: ## Cria trait
	@read -p "Nome do trait: " name; \
	docker compose exec php php artisan make:trait $$name

make-scope: ## Cria scope para model
	@read -p "Nome do scope: " name; \
	docker compose exec php php artisan make:scope $$name

make-view: ## Cria view Blade
	@read -p "Nome da view: " name; \
	docker compose exec php php artisan make:view $$name

make-config: ## Cria arquivo de configuração
	@read -p "Nome do config: " name; \
	docker compose exec php php artisan make:config $$name

make-job-middleware: ## Cria middleware para jobs
	@read -p "Nome do job middleware: " name; \
	docker compose exec php php artisan make:job-middleware $$name

##@ 📊 Logs & Monitoramento

logs-laravel: ## Tail logs do Laravel
	docker compose exec php tail -f storage/logs/laravel.log

pail: ## Tail logs usando Pail (Laravel 12+)
	docker compose exec php php artisan pail

logs-laravel-clear: ## Limpa logs do Laravel
	docker compose exec php sh -c "> storage/logs/laravel.log"
	@echo "$(GREEN)✅ Logs limpos!$(NC)"

logs-mysql-error: ## Mostra error log do MySQL
	docker compose exec mysql tail -f /var/log/mysql/error.log

logs-mysql-general: ## Mostra general log do MySQL
	docker compose exec mysql tail -f /var/lib/mysql/general.log

logs-nginx-access: ## Mostra access log do Nginx
	docker compose exec nginx tail -f /var/log/nginx/access.log

logs-nginx-error: ## Mostra error log do Nginx
	docker compose exec nginx tail -f /var/log/nginx/error.log

##@ 🔧 Manutenção

maintenance-on: ## Ativa modo de manutenção
	@echo "$(YELLOW)🚧 Ativando modo de manutenção...$(NC)"
	docker compose exec php php artisan down
	@echo "$(GREEN)✅ Modo de manutenção ativado!$(NC)"

maintenance-off: ## Desativa modo de manutenção
	@echo "$(GREEN)✅ Desativando modo de manutenção...$(NC)"
	docker compose exec php php artisan up
	@echo "$(GREEN)✅ Aplicação disponível novamente!$(NC)"

maintenance-secret: ## Ativa manutenção com secret
	@read -p "Secret (token para bypass): " secret; \
	docker compose exec php php artisan down --secret=$$secret && \
	echo "$(GREEN)Acesse com: ?secret=$$secret$(NC)"

##@ 📈 Status & Health

status: ## Mostra status completo do ambiente
	@echo "$(BLUE)━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━$(NC)"
	@echo "$(GREEN)📊 Status do Ambiente Laravel$(NC)"
	@echo "$(BLUE)━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━$(NC)"
	@echo ""
	@echo "$(YELLOW)🐳 Containers:$(NC)"
	@docker compose ps
	@echo ""
	@echo "$(YELLOW)📦 Aplicação:$(NC)"
	@docker compose exec php php artisan about
	@echo ""
	@echo "$(YELLOW)🗄️  Migrations:$(NC)"
	@docker compose exec php php artisan migrate:status

health: ## Check de saúde completo
	@echo "$(GREEN)🏥 Health Check...$(NC)"
	@echo ""
	@echo "$(YELLOW)Testando MySQL...$(NC)"
	@docker compose exec mysql mysqladmin -u root -proot ping 2>/dev/null && echo "$(GREEN)✓ MySQL OK$(NC)" || echo "$(YELLOW)⚠ MySQL não está rodando$(NC)"
	@echo ""
	@echo "$(YELLOW)Testando PostgreSQL...$(NC)"
	@docker compose exec postgres pg_isready -U developer -d db_laravel 2>/dev/null && echo "$(GREEN)✓ PostgreSQL OK$(NC)" || echo "$(YELLOW)⚠ PostgreSQL não está rodando$(NC)"
	@echo ""
	@echo "$(YELLOW)Testando MongoDB...$(NC)"
	@docker compose exec mongodb mongosh --eval "db.adminCommand('ping')" --quiet 2>/dev/null && echo "$(GREEN)✓ MongoDB OK$(NC)" || echo "$(YELLOW)⚠ MongoDB não está rodando$(NC)"
	@echo ""
	@echo "$(YELLOW)Testando Redis...$(NC)"
	@docker compose exec redis redis-cli ping 2>/dev/null && echo "$(GREEN)✓ Redis OK$(NC)" || echo "$(RED)❌ Redis com problema$(NC)"
	@echo ""
	@echo "$(YELLOW)Testando PHP-FPM...$(NC)"
	@docker compose exec php php -v | head -1 || echo "$(RED)❌ PHP com problema$(NC)"
	@echo ""
	@echo "$(GREEN)✅ Health check concluído!$(NC)"

info: ## Mostra informações do ambiente
	@echo "$(BLUE)━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━$(NC)"
	@echo "$(GREEN)ℹ️  Informações do Ambiente$(NC)"
	@echo "$(BLUE)━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━$(NC)"
	@echo ""
	@echo "$(YELLOW)🌐 URLs:$(NC)"
	@echo "  Aplicação: http://localhost:8080"
	@echo "  Mailpit:   http://localhost:32770"
	@echo ""
	@echo "$(YELLOW)🗄️  MySQL:$(NC)"
	@echo "  Host:     localhost (ou mysql dentro dos containers)"
	@echo "  Port:     3306"
	@echo "  Database: db_laravel"
	@echo "  User:     developer"
	@echo "  Password: 123456"
	@echo ""
	@echo "$(YELLOW)🐘 PostgreSQL:$(NC)"
	@echo "  Host:     localhost (ou postgres dentro dos containers)"
	@echo "  Port:     5432"
	@echo "  Database: db_laravel"
	@echo "  User:     developer"
	@echo "  Password: 123456"
	@echo ""
	@echo "$(YELLOW)🍃 MongoDB:$(NC)"
	@echo "  Host:     localhost (ou mongodb dentro dos containers)"
	@echo "  Port:     27017"
	@echo "  Database: db_laravel"
	@echo "  User:     developer"
	@echo "  Password: 123456"
	@echo "  Auth DB:  admin"
	@echo ""
	@echo "$(YELLOW)🔴 Redis:$(NC)"
	@echo "  Host: localhost (ou redis dentro dos containers)"
	@echo "  Port: 6379"
	@echo ""

##@ 🧼 Limpeza & Reset

clean: ## Limpa arquivos temporários
	@echo "$(GREEN)🧼 Limpando arquivos temporários...$(NC)"
	docker compose exec php rm -rf storage/framework/cache/*
	docker compose exec php rm -rf storage/framework/sessions/*
	docker compose exec php rm -rf storage/framework/views/*
	@echo "$(GREEN)✅ Limpeza concluída!$(NC)"

reset-hard: ## Reset completo do ambiente (⚠️ APAGA TUDO!)
	@echo "$(RED)⚠️⚠️⚠️  ATENÇÃO: Isso vai APAGAR TODOS OS DADOS! ⚠️⚠️⚠️$(NC)"
	@echo "$(RED)Isso inclui: banco de dados, cache, volumes, etc.$(NC)"
	@read -p "Digite 'CONFIRMO' para continuar: " confirm; \
	if [ "$$confirm" = "CONFIRMO" ]; then \
		echo "$(YELLOW)Parando containers...$(NC)"; \
		docker compose down -v; \
		echo "$(YELLOW)Removendo dados...$(NC)"; \
		docker compose exec php rm -rf vendor node_modules storage/logs/*; \
		echo "$(GREEN)✅ Reset completo realizado!$(NC)"; \
		echo "$(YELLOW)Execute 'make up' e 'make setup' para reconfigurar$(NC)"; \
	else \
		echo "Cancelado."; \
	fi

prune: ## Remove containers, volumes e imagens não utilizados
	@echo "$(YELLOW)🧹 Limpando Docker...$(NC)"
	docker system prune -af --volumes
	@echo "$(GREEN)✅ Docker limpo!$(NC)"

##@ 🚀 Workflows Rápidos

dev: up logs ## Inicia e mostra logs

quick-start: up setup ## Start rápido com setup

rebuild-all: down up-build migrate ## Rebuild completo

fresh: migrate-fresh-seed clear-all ## Fresh start com seed

deploy-prep: ## Prepara para deploy
	@echo "$(GREEN)🚀 Preparando para deploy...$(NC)"
	$(MAKE) test
	$(MAKE) production-ready
	@echo "$(GREEN)✅ Pronto para deploy!$(NC)"

##@ 🐛 Debug & Desenvolvimento

debug: ## Mostra informações de debug
	@echo "$(YELLOW)🐛 Debug Info:$(NC)"
	@echo ""
	docker compose exec php php -v
	@echo ""
	docker compose exec php composer --version
	@echo ""
	docker compose exec php php artisan --version

debug-config: ## Mostra configuração atual
	docker compose exec php php artisan config:show

debug-env: ## Mostra variáveis de ambiente
	docker compose exec php php artisan env

env-encrypt: ## Criptografa arquivo .env
	docker compose exec php php artisan env:encrypt

env-decrypt: ## Descriptografa arquivo .env
	@read -p "Caminho do arquivo encriptado: " path; \
	docker compose exec php php artisan env:decrypt $$path

debug-routes: ## Debug de rotas
	docker compose exec php php artisan route:list -vvv

tail-all: ## Tail de todos os logs
	docker compose logs -f

watch-logs: ## Watch logs do Laravel (requer inotify-tools)
	docker compose exec php watch -n 1 tail -n 50 storage/logs/laravel.log

##@ 📚 Tabelas Úteis

session-table: ## Cria tabela de sessões
	docker compose exec php php artisan session:table
	docker compose exec php php artisan migrate

notifications-table: ## Cria tabela de notificações
	docker compose exec php php artisan notifications:table
	docker compose exec php php artisan migrate

cache-table: ## Cria tabela de cache
	docker compose exec php php artisan cache:table
	docker compose exec php php artisan migrate

##@ 🎨 Frontend

vite-build: ## Build Vite (produção)
	docker compose exec php npm run build

vite-dev: ## Dev server Vite
	docker compose exec php npm run dev

##@ 📝 Outros

vendor-publish: ## Publica assets de vendors
	docker compose exec php php artisan vendor:publish

list-commands: ## Lista todos comandos Artisan
	docker compose exec php php artisan list

stub-publish: ## Publica stubs para customização
	docker compose exec php php artisan stub:publish

package-discover: ## Descobre packages
	docker compose exec php php artisan package:discover

ide-helper: ## Gera helpers para IDE
	docker compose exec php php artisan ide-helper:generate
	docker compose exec php php artisan ide-helper:models -N
	docker compose exec php php artisan ide-helper:meta

##@ 🔭 Laravel Telescope (Debug & Monitoring)

telescope-install: ## Instala Laravel Telescope
	docker compose exec php composer require laravel/telescope --dev
	docker compose exec php php artisan telescope:install
	docker compose exec php php artisan migrate

telescope-publish: ## Publica assets do Telescope
	docker compose exec php php artisan telescope:publish

telescope-clear: ## Limpa registros do Telescope
	docker compose exec php php artisan telescope:clear

telescope-prune: ## Remove registros antigos do Telescope
	docker compose exec php php artisan telescope:prune

telescope-pause: ## Pausa gravação do Telescope
	docker compose exec php php artisan telescope:pause

telescope-continue: ## Continua gravação do Telescope
	docker compose exec php php artisan telescope:continue

##@ 🌊 Laravel Horizon (Queue Dashboard)

horizon-install: ## Instala Laravel Horizon
	docker compose exec php composer require laravel/horizon
	docker compose exec php php artisan horizon:install
	docker compose exec php php artisan migrate

horizon: ## Inicia Horizon
	docker compose exec php php artisan horizon

horizon-daemon: ## Inicia Horizon em background
	docker compose exec -d php php artisan horizon

horizon-pause: ## Pausa workers do Horizon
	docker compose exec php php artisan horizon:pause

horizon-continue: ## Retoma workers do Horizon
	docker compose exec php php artisan horizon:continue

horizon-terminate: ## Termina Horizon gracefully
	docker compose exec php php artisan horizon:terminate

horizon-status: ## Status do Horizon
	docker compose exec php php artisan horizon:status

horizon-publish: ## Publica assets do Horizon
	docker compose exec php php artisan horizon:publish

horizon-list: ## Lista supervisors do Horizon
	docker compose exec php php artisan horizon:list

horizon-purge: ## Limpa jobs terminados/falhados
	docker compose exec php php artisan horizon:purge

##@ 🔐 Laravel Sanctum (API Authentication)

sanctum-install: ## Instala Laravel Sanctum
	docker compose exec php composer require laravel/sanctum
	docker compose exec php php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
	docker compose exec php php artisan migrate

sanctum-prune: ## Remove tokens expirados
	docker compose exec php php artisan sanctum:prune-expired

##@ 🎫 Laravel Passport (OAuth2)

passport-install: ## Instala Laravel Passport
	docker compose exec php composer require laravel/passport
	docker compose exec php php artisan migrate
	docker compose exec php php artisan passport:install

passport-keys: ## Gera chaves de encriptação
	docker compose exec php php artisan passport:keys

passport-client: ## Cria novo client OAuth2
	docker compose exec php php artisan passport:client

passport-purge: ## Remove tokens revogados/expirados
	docker compose exec php php artisan passport:purge

##@ 🔍 Laravel Scout (Full-text Search)

scout-install: ## Instala Laravel Scout
	docker compose exec php composer require laravel/scout
	docker compose exec php php artisan vendor:publish --provider="Laravel\Scout\ScoutServiceProvider"

scout-import: ## Importa registros para search index
	@read -p "Nome do Model: " model; \
	docker compose exec php php artisan scout:import "App\\Models\\$$model"

scout-flush: ## Remove todos registros do index
	@read -p "Nome do Model: " model; \
	docker compose exec php php artisan scout:flush "App\\Models\\$$model"

scout-delete-index: ## Deleta index
	@read -p "Nome do índice: " index; \
	docker compose exec php php artisan scout:delete-index $$index

scout-status: ## Status do Scout
	docker compose exec php php artisan scout:status

##@ 📡 Broadcasting & WebSockets

broadcasting-install: ## Setup de Broadcasting
	docker compose exec php php artisan install:broadcasting

reverb-install: ## Instala Laravel Reverb (WebSockets)
	docker compose exec php composer require laravel/reverb
	docker compose exec php php artisan reverb:install

reverb-start: ## Inicia servidor Reverb
	docker compose exec php php artisan reverb:start

reverb-restart: ## Reinicia servidor Reverb
	docker compose exec php php artisan reverb:restart

pusher-config: ## Configura Pusher
	@echo "$(YELLOW)Configure as variáveis no .env:$(NC)"
	@echo "BROADCAST_DRIVER=pusher"
	@echo "PUSHER_APP_ID=your-app-id"
	@echo "PUSHER_APP_KEY=your-app-key"
	@echo "PUSHER_APP_SECRET=your-app-secret"
	@echo "PUSHER_APP_CLUSTER=mt1"

##@ 🎨 Laravel Livewire

livewire-install: ## Instala Laravel Livewire
	docker compose exec php composer require livewire/livewire
	docker compose exec php php artisan livewire:publish --config

livewire-make: ## Cria componente Livewire
	@read -p "Nome do componente: " name; \
	docker compose exec php php artisan make:livewire $$name

livewire-delete: ## Remove componente Livewire
	@read -p "Nome do componente: " name; \
	docker compose exec php php artisan livewire:delete $$name

livewire-move: ## Move/renomeia componente Livewire
	@read -p "De (nome atual): " from; \
	read -p "Para (novo nome): " to; \
	docker compose exec php php artisan livewire:move $$from $$to

livewire-copy: ## Copia componente Livewire
	@read -p "De: " from; \
	read -p "Para: " to; \
	docker compose exec php php artisan livewire:copy $$from $$to

livewire-discover: ## Descobre componentes Livewire
	docker compose exec php php artisan livewire:discover

##@ 🔒 Segurança & Policies

policy-make: ## Cria policy
	@read -p "Nome da Policy: " name; \
	docker compose exec php php artisan make:policy $$name

gate-list: ## Lista gates definidos
	docker compose exec php php artisan gate:list

ability-check: ## Testa uma ability
	@echo "Use Tinker para testar: Gate::allows('ability-name', \$model)"

##@ 🗃️ Model & Eloquent Avançado

model-show: ## Mostra informações de um model
	@read -p "Nome do Model: " model; \
	docker compose exec php php artisan model:show "App\\Models\\$$model"

model-prune: ## Remove models com trait Prunable
	docker compose exec php php artisan model:prune

model-prune-dry: ## Simula remoção de models
	docker compose exec php php artisan model:prune --pretend

scope-list: ## Lista scopes de um model (requer IDE Helper)
	@read -p "Nome do Model: " model; \
	grep -n "scope" laravel/app/Models/$$model.php || echo "Nenhum scope encontrado"

##@ 🐳 Docker - Gerenciamento Avançado

docker-stats: ## Mostra uso de recursos dos containers
	docker stats --no-stream

docker-stats-live: ## Monitora recursos em tempo real
	docker stats

docker-top-php: ## Mostra processos do container PHP
	docker compose top php

docker-top-all: ## Mostra processos de todos containers
	docker compose top

docker-inspect-php: ## Inspeciona container PHP
	docker compose exec php sh -c "cat /etc/os-release && php -v && composer -V"

docker-inspect-nginx: ## Inspeciona container Nginx
	docker compose exec nginx sh -c "cat /etc/os-release && nginx -v"

docker-inspect-mysql: ## Inspeciona container MySQL
	docker compose exec mysql sh -c "mysql --version"

docker-disk: ## Mostra uso de disco do Docker
	docker system df

docker-disk-verbose: ## Uso de disco detalhado
	docker system df -v

##@ 🗂️ Docker - Volumes

volume-list: ## Lista todos os volumes
	docker volume ls

volume-inspect: ## Inspeciona volume específico
	@read -p "Nome do volume: " vol; \
	docker volume inspect $$vol

volume-prune: ## Remove volumes não utilizados
	@echo "$(RED)⚠️  ATENÇÃO: Remove volumes não utilizados!$(NC)"
	@read -p "Confirma? [s/N]: " confirm; \
	if [ "$$confirm" = "s" ] || [ "$$confirm" = "S" ]; then \
		docker volume prune; \
	fi

volume-backup: ## Backup de um volume
	@read -p "Nome do volume: " vol; \
	read -p "Nome do arquivo de backup (ex: backup.tar): " file; \
	docker run --rm -v $$vol:/source -v $$(pwd)/backups:/backup alpine tar czf /backup/$$file -C /source .

volume-restore: ## Restaura backup de volume
	@read -p "Nome do volume: " vol; \
	read -p "Nome do arquivo (em backups/): " file; \
	docker run --rm -v $$vol:/target -v $$(pwd)/backups:/backup alpine sh -c "cd /target && tar xzf /backup/$$file"

##@ 🌐 Docker - Networks

network-list: ## Lista networks
	docker network ls

network-inspect: ## Inspeciona network
	docker network inspect setup-laravel-network

network-prune: ## Remove networks não utilizadas
	docker network prune

##@ 📦 Docker - Images

image-prune: ## Remove imagens não utilizadas
	docker image prune

image-prune-all: ## Remove TODAS imagens não usadas
	@echo "$(RED)⚠️  Remove todas imagens não utilizadas!$(NC)"
	@read -p "Confirma? [s/N]: " confirm; \
	if [ "$$confirm" = "s" ] || [ "$$confirm" = "S" ]; then \
		docker image prune -a; \
	fi

image-size: ## Mostra tamanho das imagens
	docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"

image-list: ## Lista todas imagens
	docker images

image-list-dangling: ## Lista imagens órfãs (dangling)
	docker images --filter "dangling=true"

image-remove: ## Remove imagem específica
	@read -p "ID ou nome:tag da imagem: " img; \
	docker rmi $$img && echo "$(GREEN)✅ Imagem removida!$(NC)"

image-remove-dangling: ## Remove imagens órfãs (dangling)
	@echo "$(YELLOW)🗑️  Removendo imagens órfãs...$(NC)"
	docker image prune -f
	@echo "$(GREEN)✅ Imagens órfãs removidas!$(NC)"

image-remove-by-name: ## Remove imagens por nome/padrão
	@read -p "Padrão do nome (ex: setup-laravel-*): " pattern; \
	docker images --filter "reference=$$pattern" --format "{{.ID}}" | xargs -r docker rmi -f && \
	echo "$(GREEN)✅ Imagens removidas!$(NC)"

image-remove-project: ## Remove imagens do projeto atual
	@echo "$(YELLOW)🗑️  Removendo imagens do projeto...$(NC)"
	@docker compose down --rmi local 2>/dev/null || true
	@echo "$(GREEN)✅ Imagens do projeto removidas!$(NC)"

image-remove-all: ## Remove TODAS imagens (⚠️ CUIDADO!)
	@echo "$(RED)⚠️⚠️⚠️  Isso vai remover TODAS as imagens! ⚠️⚠️⚠️$(NC)"
	@read -p "Digite 'CONFIRMO' para continuar: " confirm; \
	if [ "$$confirm" = "CONFIRMO" ]; then \
		docker rmi -f $$(docker images -q) 2>/dev/null || echo "Nenhuma imagem para remover"; \
		echo "$(GREEN)✅ Todas imagens removidas!$(NC)"; \
	else \
		echo "Cancelado."; \
	fi

pull-latest: ## Atualiza imagens base
	docker compose pull

##@ 📋 Docker - Containers Avançado

container-list-all: ## Lista TODOS containers (incluindo parados)
	docker ps -a

container-list-stopped: ## Lista apenas containers parados
	docker ps -a --filter "status=exited"

container-remove-stopped: ## Remove todos containers parados
	@echo "$(YELLOW)🗑️  Removendo containers parados...$(NC)"
	docker container prune -f
	@echo "$(GREEN)✅ Containers parados removidos!$(NC)"

container-remove-all-stopped: ## Remove containers parados (interativo)
	@echo "$(YELLOW)🗑️  Containers parados:$(NC)"
	@docker ps -a --filter "status=exited" --format "table {{.ID}}\t{{.Names}}\t{{.Status}}"
	@echo ""
	@read -p "Remover todos? [s/N]: " confirm; \
	if [ "$$confirm" = "s" ] || [ "$$confirm" = "S" ]; then \
		docker container prune -f; \
		echo "$(GREEN)✅ Removidos!$(NC)"; \
	else \
		echo "Cancelado."; \
	fi

container-remove: ## Remove container específico
	@read -p "ID ou nome do container: " name; \
	docker rm -f $$name && echo "$(GREEN)✅ Container removido!$(NC)"

container-stop-all: ## Para todos containers (projeto atual)
	docker compose stop

container-remove-all: ## Remove todos containers do projeto
	@echo "$(RED)⚠️  Isso vai remover TODOS os containers do projeto!$(NC)"
	@read -p "Tem certeza? [s/N]: " confirm; \
	if [ "$$confirm" = "s" ] || [ "$$confirm" = "S" ]; then \
		docker compose down --remove-orphans; \
		echo "$(GREEN)✅ Containers removidos!$(NC)"; \
	else \
		echo "Cancelado."; \
	fi

container-remove-by-name: ## Remove containers por nome/padrão
	@read -p "Padrão do nome (ex: setup-laravel-*): " pattern; \
	docker ps -a --filter "name=$$pattern" --format "{{.ID}}" | xargs -r docker rm -f && \
	echo "$(GREEN)✅ Containers removidos!$(NC)"

container-inspect-php: ## Inspeciona detalhes do container PHP
	docker inspect setup-laravel-php

container-logs-since: ## Logs desde determinado tempo
	@read -p "Container (php/nginx/mysql/redis): " cont; \
	read -p "Tempo (ex: 10m, 1h, 2024-01-01): " time; \
	docker compose logs --since $$time $$cont

container-export: ## Exporta filesystem do container
	@read -p "Container: " cont; \
	read -p "Nome do arquivo (ex: backup.tar): " file; \
	docker export setup-laravel-$$cont > $$file

container-diff: ## Mostra mudanças no filesystem
	@read -p "Container (php/nginx/mysql/redis): " cont; \
	docker diff setup-laravel-$$cont

##@ 📂 Docker - Arquivos & Cópia

copy-to-php: ## Copia arquivo do host para container PHP
	@read -p "Arquivo local: " src; \
	read -p "Destino no container: " dest; \
	docker cp $$src setup-laravel-php:$$dest

copy-from-php: ## Copia arquivo do container PHP para host
	@read -p "Arquivo no container: " src; \
	read -p "Destino local: " dest; \
	docker cp setup-laravel-php:$$src $$dest

copy-env: ## Copia .env para dentro do container
	docker cp laravel/.env setup-laravel-php:/var/www/.env

##@ 🔧 Docker - Manutenção

docker-clean: ## Limpeza geral do Docker
	@echo "$(YELLOW)🧹 Limpando Docker...$(NC)"
	docker compose down
	docker system prune -f
	@echo "$(GREEN)✅ Limpeza concluída!$(NC)"

docker-clean-all: ## Limpeza COMPLETA (⚠️ CUIDADO!)
	@echo "$(RED)⚠️⚠️⚠️  REMOVE TUDO DO DOCKER! ⚠️⚠️⚠️$(NC)"
	@read -p "Digite 'CONFIRMO' para continuar: " confirm; \
	if [ "$$confirm" = "CONFIRMO" ]; then \
		docker compose down -v; \
		docker system prune -af --volumes; \
		echo "$(GREEN)✅ Docker completamente limpo!$(NC)"; \
	else \
		echo "Cancelado."; \
	fi

docker-clean-containers: ## Remove containers parados do sistema
	@echo "$(YELLOW)🧹 Limpando containers parados...$(NC)"
	docker container prune -f
	@echo "$(GREEN)✅ Containers limpos!$(NC)"

docker-clean-images: ## Remove imagens não utilizadas
	@echo "$(YELLOW)🧹 Limpando imagens não utilizadas...$(NC)"
	docker image prune -f
	@echo "$(GREEN)✅ Imagens limpas!$(NC)"

docker-clean-all-images: ## Remove TODAS imagens não utilizadas (com tags)
	@echo "$(RED)⚠️  Remove todas imagens não utilizadas (incluindo com tags)!$(NC)"
	@read -p "Confirma? [s/N]: " confirm; \
	if [ "$$confirm" = "s" ] || [ "$$confirm" = "S" ]; then \
		docker image prune -a -f; \
		echo "$(GREEN)✅ Imagens removidas!$(NC)"; \
	else \
		echo "Cancelado."; \
	fi

docker-clean-networks: ## Remove networks não utilizadas
	@echo "$(YELLOW)🧹 Limpando networks não utilizadas...$(NC)"
	docker network prune -f
	@echo "$(GREEN)✅ Networks limpas!$(NC)"

docker-clean-build-cache: ## Remove build cache do Docker
	@echo "$(YELLOW)🧹 Limpando build cache...$(NC)"
	docker builder prune -f
	@echo "$(GREEN)✅ Build cache limpo!$(NC)"

docker-clean-all-cache: ## Remove todo cache do Docker (build + sistema)
	@echo "$(YELLOW)🧹 Limpando todo cache...$(NC)"
	docker builder prune -af
	docker system prune -f
	@echo "$(GREEN)✅ Cache limpo!$(NC)"

docker-info: ## Mostra informações sobre uso do Docker
	@echo "$(BLUE)━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━$(NC)"
	@echo "$(GREEN)📊 Informações do Docker$(NC)"
	@echo "$(BLUE)━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━$(NC)"
	@echo ""
	@echo "$(YELLOW)💾 Uso de Disco:$(NC)"
	@docker system df
	@echo ""
	@echo "$(YELLOW)🐳 Containers (parados):$(NC)"
	@docker ps -a --filter "status=exited" --format "table {{.ID}}\t{{.Names}}\t{{.Status}}" || echo "Nenhum container parado"
	@echo ""
	@echo "$(YELLOW)🖼️  Imagens (dangling):$(NC)"
	@docker images --filter "dangling=true" --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}" || echo "Nenhuma imagem órfã"

docker-rebuild: ## Rebuild completo de todas as imagens
	docker compose down
	docker compose build --no-cache
	docker compose up -d

##@ 🎯 Workflows Completos

full-reset: ## Reset completo do projeto
	@echo "$(YELLOW)🔄 Fazendo reset completo...$(NC)"
	$(MAKE) down-volumes
	$(MAKE) up-build
	$(MAKE) setup-full
	$(MAKE) clear-all
	@echo "$(GREEN)✅ Reset completo finalizado!$(NC)"

daily-start: ## Rotina diária de início
	@echo "$(BLUE)☀️  Bom dia! Iniciando ambiente...$(NC)"
	docker compose up -d
	$(MAKE) health
	docker compose exec php php artisan migrate:status
	@echo "$(GREEN)✅ Ambiente pronto para trabalhar!$(NC)"

before-commit: ## Checklist antes de commit
	@echo "$(YELLOW)📋 Executando checklist...$(NC)"
	@echo "1️⃣  Executando testes..."
	$(MAKE) test
	@echo "2️⃣  Validando código..."
	docker compose exec php ./vendor/bin/phpstan analyse || true
	docker compose exec php ./vendor/bin/php-cs-fixer fix --dry-run || true
	@echo "3️⃣  Verificando migrations..."
	docker compose exec php php artisan migrate:status
	@echo "$(GREEN)✅ Checklist concluído!$(NC)"

deploy-production: ## Workflow de deploy para produção
	@echo "$(RED)🚀 Deploy para PRODUÇÃO$(NC)"
	@read -p "Confirma deploy? [s/N]: " confirm; \
	if [ "$$confirm" = "s" ] || [ "$$confirm" = "S" ]; then \
		echo "$(YELLOW)1/6 Executando testes...$(NC)"; \
		$(MAKE) test; \
		echo "$(YELLOW)2/6 Fazendo backup...$(NC)"; \
		$(MAKE) backup-db; \
		echo "$(YELLOW)3/6 Atualizando dependências...$(NC)"; \
		$(MAKE) composer-install; \
		echo "$(YELLOW)4/6 Executando migrations...$(NC)"; \
		$(MAKE) migrate; \
		echo "$(YELLOW)5/6 Otimizando...$(NC)"; \
		$(MAKE) production-ready; \
		echo "$(YELLOW)6/6 Reiniciando serviços...$(NC)"; \
		$(MAKE) restart; \
		echo "$(GREEN)✅ Deploy concluído com sucesso!$(NC)"; \
	else \
		echo "Deploy cancelado."; \
	fi

##@ 📊 Análise & Relatórios

analyze-code: ## Análise estática de código (PHPStan)
	docker compose exec php ./vendor/bin/phpstan analyse --memory-limit=2G

fix-code-style: ## Corrige estilo de código (PHP-CS-Fixer)
	docker compose exec php ./vendor/bin/php-cs-fixer fix

check-code-style: ## Verifica estilo sem modificar
	docker compose exec php ./vendor/bin/php-cs-fixer fix --dry-run --diff

security-check: ## Verifica vulnerabilidades
	docker compose exec php composer audit

dependencies-licenses: ## Mostra licenças das dependências
	docker compose exec php composer licenses

project-stats: ## Estatísticas do projeto
	@echo "$(BLUE)📊 Estatísticas do Projeto$(NC)"
	@echo ""
	@echo "$(YELLOW)📁 Arquivos PHP:$(NC)"
	@find laravel/app -name "*.php" 2>/dev/null | wc -l
	@echo ""
	@echo "$(YELLOW)📝 Linhas de código:$(NC)"
	@find laravel/app -name "*.php" -exec cat {} \; 2>/dev/null | wc -l
	@echo ""
	@echo "$(YELLOW)🧪 Arquivos de teste:$(NC)"
	@find laravel/tests -name "*.php" 2>/dev/null | wc -l
	@echo ""
	@echo "$(YELLOW)📦 Dependências Composer:$(NC)"
	@docker compose exec php composer show --installed | wc -l

routes-api: ## Lista apenas rotas da API
	docker compose exec php php artisan route:list --path=api

routes-web: ## Lista apenas rotas web
	docker compose exec php php artisan route:list --path=/

routes-count: ## Conta total de rotas
	@docker compose exec php php artisan route:list --json | grep -c '"uri"'

##@ 🏗️ Montagem de Ambiente Automática

check-structure: ## Verifica e cria estrutura de pastas necessárias
	@echo "$(YELLOW)🔍 Verificando estrutura...$(NC)"
	@mkdir -p laravel/storage/logs
	@mkdir -p laravel/storage/framework/cache
	@mkdir -p laravel/storage/framework/sessions
	@mkdir -p laravel/storage/framework/views
	@mkdir -p laravel/bootstrap/cache
	@mkdir -p backups
	@echo "$(GREEN)✅ Estrutura verificada!$(NC)"

create-env-from-compose: ## Cria .env baseado no docker-compose.yml
	@echo "$(YELLOW)📝 Criando .env a partir do docker-compose.yml...$(NC)"
	@if [ ! -f laravel/.env ]; then \
		cp laravel/.env.example laravel/.env; \
		echo "" >> laravel/.env; \
		echo "# Configurações dos Containers" >> laravel/.env; \
		echo "DB_HOST=mysql" >> laravel/.env; \
		echo "DB_PORT=3306" >> laravel/.env; \
		echo "DB_DATABASE=db_laravel" >> laravel/.env; \
		echo "DB_USERNAME=developer" >> laravel/.env; \
		echo "DB_PASSWORD=123456" >> laravel/.env; \
		echo "REDIS_HOST=redis" >> laravel/.env; \
		echo "REDIS_PORT=6379" >> laravel/.env; \
		echo "MAIL_MAILER=smtp" >> laravel/.env; \
		echo "MAIL_HOST=mailer" >> laravel/.env; \
		echo "MAIL_PORT=1025" >> laravel/.env; \
		echo "MAIL_ENCRYPTION=null" >> laravel/.env; \
		echo "$(GREEN)✅ .env criado com configurações dos containers!$(NC)"; \
	else \
		echo "$(YELLOW).env já existe$(NC)"; \
	fi

sync-env-with-compose: ## Sincroniza .env existente com docker-compose.yml
	@echo "$(YELLOW)🔄 Sincronizando .env com containers...$(NC)"
	@if [ -f laravel/.env ]; then \
		sed -i.bak 's/^DB_HOST=.*/DB_HOST=mysql/' laravel/.env; \
		sed -i.bak 's/^DB_DATABASE=.*/DB_DATABASE=db_laravel/' laravel/.env; \
		sed -i.bak 's/^DB_USERNAME=.*/DB_USERNAME=developer/' laravel/.env; \
		sed -i.bak 's/^DB_PASSWORD=.*/DB_PASSWORD=123456/' laravel/.env; \
		sed -i.bak 's/^REDIS_HOST=.*/REDIS_HOST=redis/' laravel/.env; \
		sed -i.bak 's/^MAIL_HOST=.*/MAIL_HOST=mailer/' laravel/.env; \
		rm -f laravel/.env.bak; \
		echo "$(GREEN)✅ .env sincronizado!$(NC)"; \
	else \
		echo "$(RED)❌ .env não encontrado!$(NC)"; \
		echo "$(YELLOW)Use: make create-env-from-compose$(NC)"; \
	fi

environment-setup: ## Setup completo do ambiente (containers + Laravel)
	@echo "$(BLUE)━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━$(NC)"
	@echo "$(GREEN)🏗️  Montando Ambiente Completo$(NC)"
	@echo "$(BLUE)━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━$(NC)"
	@echo ""
	@echo "$(YELLOW)📦 1/8 Verificando estrutura de pastas...$(NC)"
	$(MAKE) check-structure
	@echo ""
	@echo "$(YELLOW)🐳 2/8 Subindo containers...$(NC)"
	docker compose up -d
	@echo ""
	@echo "$(YELLOW)⏳ 3/8 Aguardando containers ficarem prontos...$(NC)"
	@sleep 10
	$(MAKE) health
	@echo ""
	@echo "$(YELLOW)📝 4/8 Configurando .env...$(NC)"
	$(MAKE) create-env-from-compose
	@echo ""
	@echo "$(YELLOW)📦 5/8 Instalando dependências...$(NC)"
	docker compose exec php composer install
	@echo ""
	@echo "$(YELLOW)🔑 6/8 Gerando chave da aplicação...$(NC)"
	docker compose exec php php artisan key:generate
	@echo ""
	@echo "$(YELLOW)🗄️  7/8 Configurando banco de dados...$(NC)"
	docker compose exec php php artisan migrate
	@echo ""
	@echo "$(YELLOW)🔗 8/8 Configurando storage...$(NC)"
	docker compose exec php php artisan storage:link
	@echo ""
	@echo "$(GREEN)✅ Ambiente montado com sucesso!$(NC)"
	@echo ""
	$(MAKE) info

auto-config: ## Configura automaticamente todos os serviços
	@echo "$(GREEN)⚙️  Configuração automática dos serviços...$(NC)"
	@echo ""
	@echo "$(YELLOW)🐘 Configurando PHP...$(NC)"
	@docker compose exec php php -v | head -1
	@echo ""
	@echo "$(YELLOW)🗄️  Testando MySQL...$(NC)"
	@docker compose exec mysql mysqladmin -u root -proot ping && echo "$(GREEN)✓ MySQL OK$(NC)" || echo "$(RED)✗ MySQL com problema$(NC)"
	@echo ""
	@echo "$(YELLOW)🐘 Testando PostgreSQL...$(NC)"
	@docker compose exec postgres pg_isready -U developer -d db_laravel && echo "$(GREEN)✓ PostgreSQL OK$(NC)" || echo "$(RED)✗ PostgreSQL com problema$(NC)"
	@echo ""
	@echo "$(YELLOW)🍃 Testando MongoDB...$(NC)"
	@docker compose exec mongodb mongosh --eval "db.adminCommand('ping')" --quiet && echo "$(GREEN)✓ MongoDB OK$(NC)" || echo "$(RED)✗ MongoDB com problema$(NC)"
	@echo ""
	@echo "$(YELLOW)🔴 Testando Redis...$(NC)"
	@docker compose exec redis redis-cli ping && echo "$(GREEN)✓ Redis OK$(NC)" || echo "$(RED)✗ Redis com problema$(NC)"
	@echo ""
	@echo "$(YELLOW)🌐 Testando Nginx...$(NC)"
	@docker compose exec nginx nginx -t 2>&1 && echo "$(GREEN)✓ Nginx OK$(NC)" || echo "$(RED)✗ Nginx com problema$(NC)"
	@echo ""
	@echo "$(GREEN)✅ Configuração concluída!$(NC)"

validate-containers: ## Valida se todos containers estão configurados corretamente
	@echo "$(YELLOW)🔍 Validando containers...$(NC)"
	@echo ""
	@echo "$(BLUE)Container PHP:$(NC)"
	@docker compose exec php php --version | head -1
	@docker compose exec php composer --version | head -1
	@echo ""
	@echo "$(BLUE)Container MySQL:$(NC)"
	@docker compose exec mysql mysql --version
	@docker compose exec mysql mysql -u root -proot -e "SELECT VERSION();" 2>/dev/null || echo "$(RED)Erro ao conectar$(NC)"
	@echo ""
	@echo "$(BLUE)Container PostgreSQL:$(NC)"
	@docker compose exec postgres postgres --version 2>/dev/null || echo "$(RED)Container não está rodando$(NC)"
	@echo ""
	@echo "$(BLUE)Container MongoDB:$(NC)"
	@docker compose exec mongodb mongod --version 2>/dev/null | head -1 || echo "$(RED)Container não está rodando$(NC)"
	@docker compose exec mongodb mongosh --version 2>/dev/null || echo "$(RED)Erro ao verificar$(NC)"
	@echo ""
	@echo "$(BLUE)Container Redis:$(NC)"
	@docker compose exec redis redis-server --version
	@docker compose exec redis redis-cli info server | grep redis_version
	@echo ""
	@echo "$(BLUE)Container Nginx:$(NC)"
	@docker compose exec nginx nginx -v 2>&1
	@echo ""
	@echo "$(GREEN)✅ Validação concluída!$(NC)"

show-container-config: ## Mostra configuração atual dos containers
	@echo "$(BLUE)━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━$(NC)"
	@echo "$(GREEN)📋 Configuração dos Containers$(NC)"
	@echo "$(BLUE)━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━$(NC)"
	@echo ""
	@echo "$(YELLOW)🐘 PHP:$(NC)"
	@echo "  Container: setup-laravel-php"
	@echo "  Volume: ./laravel → /var/www"
	@docker compose exec php php -r "echo '  PHP Version: ' . PHP_VERSION . PHP_EOL;" 2>/dev/null || echo "  Container não está rodando"
	@echo ""
	@echo "$(YELLOW)🗄️  MySQL:$(NC)"
	@echo "  Container: setup-laravel-mysql"
	@echo "  Database: db_laravel"
	@echo "  Port: 3306"
	@echo "  User: developer / root"
	@docker compose exec mysql mysql --version 2>/dev/null || echo "  Container não está rodando"
	@echo ""
	@echo "$(YELLOW)🐘 PostgreSQL:$(NC)"
	@echo "  Container: setup-laravel-postgres"
	@echo "  Database: db_laravel"
	@echo "  Port: 5432"
	@echo "  User: developer / postgres"
	@docker compose exec postgres postgres --version 2>/dev/null || echo "  Container não está rodando"
	@echo ""
	@echo "$(YELLOW)🍃 MongoDB:$(NC)"
	@echo "  Container: setup-laravel-mongodb"
	@echo "  Database: db_laravel"
	@echo "  Port: 27017"
	@echo "  User: developer / root"
	@docker compose exec mongodb mongod --version 2>/dev/null | head -1 || echo "  Container não está rodando"
	@echo ""
	@echo "$(YELLOW)🔴 Redis:$(NC)"
	@echo "  Container: setup-laravel-redis"
	@echo "  Port: 6379"
	@docker compose exec redis redis-cli INFO server 2>/dev/null || echo "  Container não está rodando"
	@echo ""
	@echo "$(YELLOW)🌐 Nginx:$(NC)"
	@echo "  Container: setup-laravel-nginx"
	@echo "  Ports: 8080:80, 443:443"
	@docker compose exec nginx nginx -v 2>&1 || echo "  Container não está rodando"
	@echo ""
	@echo "$(YELLOW)📧 Mailpit:$(NC)"
	@echo "  Container: setup-laravel-mailer"
	@echo "  Web UI: http://localhost:32770"
	@echo "  SMTP: 1025"

inspect-compose: ## Analisa docker-compose.yml e mostra serviços
	@echo "$(BLUE)📋 Serviços definidos no docker-compose.yml:$(NC)"
	@echo ""
	@docker compose config --services
	@echo ""
	@echo "$(BLUE)🔍 Detalhes dos containers:$(NC)"
	@docker compose ps --format "table {{.Name}}\t{{.Status}}\t{{.Ports}}"
	@echo ""
	@echo "$(BLUE)📊 Networks:$(NC)"
	@docker compose config --format json | grep -o '"setup-laravel-network"' | uniq || echo "setup-laravel-network"
	@echo ""
	@echo "$(BLUE)💾 Volumes:$(NC)"
	@docker compose config --volumes

init-project: ## Inicializa projeto do zero (primeira vez) 🌟
	@echo "$(BLUE)━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━$(NC)"
	@echo "$(GREEN)🎉 Inicializando Projeto Laravel com Docker$(NC)"
	@echo "$(BLUE)━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━$(NC)"
	@echo ""
	@echo "$(YELLOW)Este comando vai:$(NC)"
	@echo "  1. Criar estrutura de pastas"
	@echo "  2. Construir e iniciar containers"
	@echo "  3. Configurar .env com dados dos containers"
	@echo "  4. Instalar dependências"
	@echo "  5. Configurar Laravel"
	@echo "  6. Executar migrations"
	@echo ""
	@read -p "Continuar? [s/N]: " confirm; \
	if [ "$$confirm" = "s" ] || [ "$$confirm" = "S" ]; then \
		$(MAKE) environment-setup; \
		echo ""; \
		echo "$(GREEN)🎉 Projeto inicializado com sucesso!$(NC)"; \
		echo ""; \
		echo "$(YELLOW)Acesse:$(NC)"; \
		echo "  Aplicação: http://localhost:8080"; \
		echo "  Mailpit: http://localhost:32770"; \
		echo ""; \
		echo "$(YELLOW)Comandos úteis:$(NC)"; \
		echo "  make help        - Ver todos comandos"; \
		echo "  make bash        - Acessar container PHP"; \
		echo "  make db          - Acessar MySQL"; \
		echo "  make logs        - Ver logs"; \
	else \
		echo "Cancelado."; \
	fi

verify-environment: ## Verifica se o ambiente está pronto para uso
	@echo "$(BLUE)🔍 Verificando ambiente...$(NC)"
	@echo ""
	@echo "$(YELLOW)1. Verificando Docker...$(NC)"
	@docker --version || (echo "$(RED)Docker não instalado$(NC)" && exit 1)
	@echo "$(GREEN)✓ Docker OK$(NC)"
	@echo ""
	@echo "$(YELLOW)2. Verificando Docker Compose...$(NC)"
	@docker compose version || (echo "$(RED)Docker Compose não instalado$(NC)" && exit 1)
	@echo "$(GREEN)✓ Docker Compose OK$(NC)"
	@echo ""
	@echo "$(YELLOW)3. Verificando containers...$(NC)"
	@docker compose ps | grep -q "Up" && echo "$(GREEN)✓ Containers rodando$(NC)" || echo "$(YELLOW)! Containers não estão rodando (use: make up)$(NC)"
	@echo ""
	@echo "$(YELLOW)4. Verificando arquivos...$(NC)"
	@[ -f "docker-compose.yml" ] && echo "$(GREEN)✓ docker-compose.yml OK$(NC)" || echo "$(RED)✗ docker-compose.yml não encontrado$(NC)"
	@[ -f "laravel/.env" ] && echo "$(GREEN)✓ .env OK$(NC)" || echo "$(YELLOW)! .env não encontrado (use: make create-env-from-compose)$(NC)"
	@[ -d "laravel/vendor" ] && echo "$(GREEN)✓ Dependências instaladas$(NC)" || echo "$(YELLOW)! Vendor não encontrado (use: make install)$(NC)"
	@echo ""
	@echo "$(GREEN)✅ Verificação concluída!$(NC)"

fix-permissions-auto: ## Corrige permissões automaticamente
	@echo "$(YELLOW)🔐 Corrigindo permissões...$(NC)"
	docker compose exec php chmod -R 775 storage bootstrap/cache 2>/dev/null || true
	docker compose exec php chown -R www-data:www-data storage bootstrap/cache 2>/dev/null || true
	@echo "$(GREEN)✅ Permissões corrigidas!$(NC)"

container-health-check: ## Verifica saúde de cada container individualmente
	@echo "$(BLUE)🏥 Health Check Individual dos Containers$(NC)"
	@echo ""
	@echo "$(YELLOW)🐘 PHP-FPM:$(NC)"
	@docker compose exec php php-fpm -t 2>&1 | grep -q "configuration file" && echo "$(GREEN)✓ Healthy$(NC)" || echo "$(RED)✗ Unhealthy$(NC)"
	@echo ""
	@echo "$(YELLOW)🗄️  MySQL:$(NC)"
	@docker compose exec mysql mysqladmin -u root -proot ping 2>/dev/null | grep -q "alive" && echo "$(GREEN)✓ Healthy$(NC)" || echo "$(RED)✗ Unhealthy$(NC)"
	@echo ""
	@echo "$(YELLOW)🐘 PostgreSQL:$(NC)"
	@docker compose exec postgres pg_isready -U developer -d db_laravel 2>/dev/null && echo "$(GREEN)✓ Healthy$(NC)" || echo "$(RED)✗ Unhealthy$(NC)"
	@echo ""
	@echo "$(YELLOW)🍃 MongoDB:$(NC)"
	@docker compose exec mongodb mongosh --eval "db.adminCommand('ping')" --quiet 2>/dev/null && echo "$(GREEN)✓ Healthy$(NC)" || echo "$(RED)✗ Unhealthy$(NC)"
	@echo ""
	@echo "$(YELLOW)🔴 Redis:$(NC)"
	@docker compose exec redis redis-cli ping 2>/dev/null | grep -q "PONG" && echo "$(GREEN)✓ Healthy$(NC)" || echo "$(RED)✗ Unhealthy$(NC)"
	@echo ""
	@echo "$(YELLOW)🌐 Nginx:$(NC)"
	@docker compose exec nginx nginx -t 2>&1 | grep -q "successful" && echo "$(GREEN)✓ Healthy$(NC)" || echo "$(RED)✗ Unhealthy$(NC)"

##@ 🎯 Aliases Úteis

dev-mode: clear-all optimize-clear ## Modo desenvolvimento (sem cache)
	@echo "$(GREEN)✅ Modo desenvolvimento ativado!$(NC)"

prod-mode: production-ready ## Modo produção (com cache)
	@echo "$(GREEN)✅ Modo produção ativado!$(NC)"

fresh-install: down up setup-full ## Instalação limpa completa
	@echo "$(GREEN)✅ Instalação limpa concluída!$(NC)"

##@ 🔀 Git - Status & Informações

git-status: ## Mostra status do repositório
	git status

git-status-short: ## Status em formato curto
	git status -s

git-log: ## Mostra histórico de commits
	git log --oneline --graph --decorate -20

git-log-full: ## Histórico completo
	git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit

git-branch: ## Lista branches
	git branch -a

git-branch-verbose: ## Lista branches com detalhes
	git branch -vv

git-remote: ## Mostra repositórios remotos
	git remote -v

git-diff: ## Mostra diferenças não commitadas
	git diff

git-diff-staged: ## Mostra diferenças no staging
	git diff --staged

git-show: ## Mostra último commit
	git show

git-stash-list: ## Lista stashes
	git stash list

git-config: ## Mostra configuração do Git
	git config --list

##@ 🔀 Git - Adicionar & Commit

git-add: ## Adiciona todos arquivos modificados
	git add .

git-add-interactive: ## Adiciona arquivos interativamente
	git add -i

git-add-patch: ## Adiciona mudanças em patches
	git add -p

git-commit: ## Faz commit (solicita mensagem)
	@read -p "Mensagem do commit: " msg; \
	git commit -m "$$msg"

git-commit-amend: ## Corrige último commit (sem alterar mensagem)
	git commit --amend --no-edit

git-commit-amend-msg: ## Corrige último commit e mensagem
	@read -p "Nova mensagem: " msg; \
	git commit --amend -m "$$msg"

git-commit-all: ## Adiciona tudo e faz commit
	@read -p "Mensagem do commit: " msg; \
	git add . && git commit -m "$$msg"

##@ 🔀 Git - Branch

git-branch-create: ## Cria nova branch
	@read -p "Nome da branch: " branch; \
	git checkout -b $$branch

git-branch-switch: ## Muda para outra branch
	@read -p "Nome da branch: " branch; \
	git checkout $$branch

git-branch-delete: ## Deleta branch local
	@read -p "Nome da branch: " branch; \
	git branch -d $$branch

git-branch-delete-force: ## Deleta branch local (força)
	@read -p "Nome da branch: " branch; \
	git branch -D $$branch

git-branch-rename: ## Renomeia branch atual
	@read -p "Novo nome: " newname; \
	git branch -m $$newname

git-branch-merge: ## Faz merge de uma branch
	@read -p "Branch para mergear: " branch; \
	git merge $$branch

##@ 🔀 Git - Remote & Push/Pull

git-push: ## Faz push para origin
	@read -p "Nome da branch (padrão: atual): " branch; \
	git push origin $${branch:-$$(git branch --show-current)}

git-push-upstream: ## Push e configura upstream
	git push -u origin $$(git branch --show-current)

git-pull: ## Faz pull do origin
	git pull origin $$(git branch --show-current)

git-pull-rebase: ## Faz pull com rebase
	git pull --rebase origin $$(git branch --show-current)

git-fetch: ## Busca mudanças sem merge
	git fetch

git-fetch-all: ## Busca de todos os remotos
	git fetch --all

git-push-force: ## Push forçado (⚠️ CUIDADO!)
	@echo "$(RED)⚠️  ATENÇÃO: Push forçado pode sobrescrever histórico!$(NC)"
	@read -p "Tem certeza? [s/N]: " confirm; \
	if [ "$$confirm" = "s" ] || [ "$$confirm" = "S" ]; then \
		git push --force origin $$(git branch --show-current); \
	else \
		echo "Cancelado."; \
	fi

##@ 🔀 Git - Stash

git-stash: ## Salva mudanças temporariamente
	git stash

git-stash-message: ## Stash com mensagem
	@read -p "Mensagem do stash: " msg; \
	git stash push -m "$$msg"

git-stash-pop: ## Restaura último stash e remove
	git stash pop

git-stash-apply: ## Restaura último stash (mantém stash)
	git stash apply

git-stash-drop: ## Remove último stash
	git stash drop

git-stash-clear: ## Remove todos os stashes
	@echo "$(RED)⚠️  Isso vai remover TODOS os stashes!$(NC)"
	@read -p "Tem certeza? [s/N]: " confirm; \
	if [ "$$confirm" = "s" ] || [ "$$confirm" = "S" ]; then \
		git stash clear; \
		echo "$(GREEN)✅ Stashes removidos!$(NC)"; \
	else \
		echo "Cancelado."; \
	fi

##@ 🔀 Git - Reset & Revert

git-reset-soft: ## Reset soft (mantém mudanças no staging)
	@read -p "Quantos commits voltar? " count; \
	git reset --soft HEAD~$$count

git-reset-mixed: ## Reset mixed (mantém mudanças, remove staging)
	@read -p "Quantos commits voltar? " count; \
	git reset --mixed HEAD~$$count

git-reset-hard: ## Reset hard (⚠️ APAGA MUDANÇAS!)
	@echo "$(RED)⚠️⚠️⚠️  ATENÇÃO: Isso vai APAGAR todas as mudanças! ⚠️⚠️⚠️$(NC)"
	@read -p "Quantos commits voltar? " count; \
	read -p "Digite 'CONFIRMO' para continuar: " confirm; \
	if [ "$$confirm" = "CONFIRMO" ]; then \
		git reset --hard HEAD~$$count; \
		echo "$(GREEN)✅ Reset realizado!$(NC)"; \
	else \
		echo "Cancelado."; \
	fi

git-revert: ## Reverte último commit (cria novo commit)
	git revert HEAD --no-edit

git-clean: ## Remove arquivos não rastreados
	git clean -n

git-clean-force: ## Remove arquivos não rastreados (⚠️ CUIDADO!)
	@echo "$(RED)⚠️  Isso vai remover arquivos não rastreados!$(NC)"
	@read -p "Tem certeza? [s/N]: " confirm; \
	if [ "$$confirm" = "s" ] || [ "$$confirm" = "S" ]; then \
		git clean -fd; \
		echo "$(GREEN)✅ Arquivos removidos!$(NC)"; \
	else \
		echo "Cancelado."; \
	fi

##@ 🔀 Git - Tags

git-tag: ## Lista tags
	git tag

git-tag-create: ## Cria tag
	@read -p "Nome da tag: " tag; \
	read -p "Mensagem (opcional): " msg; \
	if [ -z "$$msg" ]; then \
		git tag $$tag; \
	else \
		git tag -a $$tag -m "$$msg"; \
	fi

git-tag-push: ## Envia tags para remote
	git push origin --tags

git-tag-delete: ## Deleta tag local
	@read -p "Nome da tag: " tag; \
	git tag -d $$tag

git-tag-delete-remote: ## Deleta tag no remote
	@read -p "Nome da tag: " tag; \
	git push origin :refs/tags/$$tag

##@ 🔀 Git - Workflow Completo

git-quick-push: ## Workflow rápido: add, commit e push
	@read -p "Mensagem do commit: " msg; \
	git add . && git commit -m "$$msg" && git push origin $$(git branch --show-current) && \
	echo "$(GREEN)✅ Mudanças enviadas!$(NC)"

git-sync: ## Sincroniza com remote (fetch + pull)
	git fetch && git pull origin $$(git branch --show-current)

git-update: ## Atualiza tudo (fetch all + pull)
	git fetch --all && git pull

.DEFAULT_GOAL := help
