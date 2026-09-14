# Exorails skills

Procedures that teach an agent to use the systems an [Exorails](https://exorails.net) environment exposes: PostgreSQL, MySQL, SQL Server, ClickHouse, Redis, MongoDB, Elasticsearch, SSH, Kubernetes, Prometheus, S3, REST and GraphQL APIs, plus how to connect an agent and reach private networks.

Each skill is a folder under `skills/` with a `SKILL.md`, in the format the open agent-skills ecosystem uses.

## Install on your machine

```
npx skills add thomasdseao/exorails-skills
```

Or one skill:

```
npx skills add thomasdseao/exorails-skills --skill postgres
```

## Or install nothing

The same skills are served to your agent through your environment's URL: open the environment in the Exorails console, tab **Skills**, and tick the ones you want. They appear in the agent's tool list as `skill__<name>` and are loaded on demand.

## Source

This repository is a mirror of `backend/internal/skills/content` in the Exorails codebase, published on every release. Improvements are welcome there.
