# gh-sdd-ai-workflow

Metodologie pro Spec-Driven Development s AI agenty a GitHub Issues.

## O tomto repozitáři

Toto je **metodologický repozitář**, ne runtime závislost. Obsahuje:
- `README.md` - kompletní dokumentace metodologie
- `skills/` - custom skills pro Claude Code
- `templates/` - issue templates pro kopírování do projektů
- `BOOTSTRAP.md` - checklist pro nastavení nového projektu

## Práce s tímto repozitářem

### Úpravy metodologie

Při úpravách `README.md`:
1. Udržuj konzistenci mezi dokumentací a soubory v `skills/` a `templates/`
2. Pokud měníš template nebo skill, uprav i odpovídající sekci v dokumentaci

### Struktura

```
├── README.md                # Hlavní dokumentace
├── CLAUDE.md                # Tento soubor
├── BOOTSTRAP.md             # Checklist pro nové projekty
├── skills/                  # Claude Code skills
│   ├── feedback/SKILL.md
│   ├── start-work/SKILL.md
│   ├── progress/SKILL.md
│   ├── done/SKILL.md
│   └── feature-spec/SKILL.md
└── templates/
    └── .github/ISSUE_TEMPLATE/
        ├── feature.md
        ├── bug.md
        └── feedback.md
```

## Použití pro nový projekt

Viz `BOOTSTRAP.md` nebo sekce "Bootstrap New Project" v `README.md`.

Rychlý příkaz:
```bash
# V novém projektu
specify init . --here --ai claude
cp -r /workspace/personal/gh-sdd-ai-workflow/templates/.github .
# + vytvořit labels, CLAUDE.md
```

## Konvence

- **Jazyk dokumentace:** čeština
- **Jazyk kódu a commitů:** angličtina
- **Skills:** anglicky (pro kompatibilitu s Claude Code)
