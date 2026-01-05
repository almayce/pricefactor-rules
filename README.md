# PriceFactor Rules Repository

Pricing rulesets for PriceFactor landing site calculator.

## Structure

- `landing-site/` - Rulesets for landing page pricing calculator
  - 7 JSON files with pricing rules for different categories
- `schemas/` - JSON schemas for ruleset validation (optional)

## Versioning

We use semantic versioning (vMAJOR.MINOR.PATCH):

- **MAJOR**: Breaking changes in rule structure (e.g., schema changes)
- **MINOR**: New rules or categories added
- **PATCH**: Bug fixes in existing rules, value adjustments

## Making Changes

### 1. Create a feature branch

```bash
git checkout -b feature/update-base-pricing
```

### 2. Edit rulesets

Edit JSON files in `landing-site/` directory.

**Important**: Validate JSON syntax before committing:

```bash
# Validate JSON
python -m json.tool landing-site/base-parameters-ruleset.json

# Or use jq
jq '.' landing-site/base-parameters-ruleset.json
```

### 3. Commit changes

```bash
git add .
git commit -m "Update base pricing multipliers"
git push origin feature/update-base-pricing
```

### 4. Create Pull Request

1. Go to GitHub repository
2. Click "Compare & pull request"
3. Review changes
4. Merge to main branch

### 5. Create Release Tag

After merging to main:

```bash
git checkout main
git pull origin main
git tag -a v1.0.1 -m "Updated base pricing multipliers"
git push origin v1.0.1
```

### 6. Update PriceFactor Application

Go to Vercel Dashboard:
1. Navigate to Project Settings → Environment Variables
2. Update `RULESET_VERSION` to `v1.0.1`
3. Redeploy application (or wait for webhook to trigger)

## Testing Changes Locally

Before creating a release tag, test changes with PriceFactor:

### Option A: Test with main branch

In Vercel preview deployment, set:
```
RULESET_VERSION=main
```

### Option B: Test with feature branch

```
RULESET_VERSION=feature/update-base-pricing
```

## Current Version

Check the latest tag:
```bash
git tag -l
```

## Rollback

If a ruleset version causes issues:

1. Identify previous stable version (e.g., `v1.0.0`)
2. In Vercel Dashboard, change `RULESET_VERSION` to `v1.0.0`
3. Redeploy application

## Rule Format

Each ruleset JSON file follows this structure:

```json
{
  "categories": {
    "category_name": {
      "stop_on_first": false,
      "rules": [
        {
          "id": "unique_rule_id",
          "priority": 10,
          "conditions": {
            "$and": [
              { "path.to.field": {"$equals": "value"} }
            ]
          },
          "action": "add",
          "metadata": {
            "description": "Rule description",
            "value": 100
          }
        }
      ]
    }
  }
}
```

### Supported Actions

- `add`: Add value to price
- `subtract`: Subtract value from price
- `multiply`: Multiply price by value
- `multiply_path`: Multiply value from payload path by value and add to price
- `warning`: Show warning (doesn't affect price)

### Supported Conditions

- `$equals`, `$not-equals`
- `$greater-than`, `$less-than`
- `$greater-than-or-equals`, `$less-than-or-equals`
- `$in`, `$not-in`
- `$and`, `$or`

## License

Private project - All rights reserved
