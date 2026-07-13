# Ansible Production Deployment Project

This project demonstrates a real-world, production-grade Ansible deployment using:
- Role-based architecture
- Multi-tier setup (Web, App, DB)
- Idempotent automation
- Environment isolation (Dev/Prod)

## Architecture Diagram

```mermaid
flowchart TD
	A[Ansible Control Node] --> B[ansible.cfg]
	A --> C[inventories/dev]
	A --> D[inventories/prod]
	A --> E[playbooks/site.yml]

	E --> F[common role on all hosts]
	E --> G[db.yml -> mysql role]
	E --> H[app.yml -> app role]
	E --> I[web.yml -> nginx role]

	C --> J[web: web1]
	C --> K[app: app1]
	C --> L[db: db1]

	D --> M[web: prod-web1]
	D --> N[app: prod-app1]
	D --> O[db: prod-db1]

	J --> K
	K --> L
	M --> N
	N --> O
```

## Deployment Sequence Diagram

```mermaid
sequenceDiagram
    participant U as Operator
    participant A as Ansible Control Node
    participant INV as Inventory (dev/prod)
    participant ALL as All Hosts
    participant DB as DB Hosts
    participant APP as App Hosts
    participant WEB as Web Hosts

    U->>A: Run ansible-playbook playbooks/site.yml
    A->>INV: Load target inventory
    INV-->>A: Return host groups (web/app/db)

    A->>ALL: Apply common role
    ALL-->>A: Common baseline configured

    A->>DB: Import and run db.yml (mysql role)
    DB-->>A: Database ready

    A->>APP: Import and run app.yml (app role)
    APP-->>A: Application ready

    A->>WEB: Import and run web.yml (nginx role)
    WEB-->>A: Web tier ready

    A-->>U: Deployment completed
```

## Run in Dev
ansible-playbook playbooks/site.yml

## Dry Run
ansible-playbook playbooks/site.yml --check

## Run in Prod
ansible-playbook -i inventories/prod/hosts playbooks/site.yml