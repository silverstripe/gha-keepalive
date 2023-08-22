# GitHub Actions - Keepalive

GitHub Action - Periodically re-enable all repository cron workflows that would otherwise auto-disable after 60 days of no repository activity - see [GitHub docs](https://docs.github.com/en/actions/learn-github-actions/usage-limits-billing-and-administration#disabling-and-enabling-workflows).

## Usage

There are no inputs for configuring this action - just define your schedule so that it runs every 60 days or less (in the below example it is run on the first of each month).

**.github/workflows/keepalive.yml**
```yml
name: Keepalive

on:
  # Run on a schedule of once per month
  schedule:
    - cron: '0 0 1 * *'
  workflow_dispatch:

jobs:
  keepalive:
    name: Keepalive
    # Only run the cron on the account hosting this repository, not on the accounts of forks
    # Change '<account_name>' to match the name of the account hosting this repository
    if: (github.event_name == 'schedule' && github.repository_owner == '<account_name>') || (github.event_name != 'schedule')
    runs-on: ubuntu-latest
    steps:
      - name: Keepalive
        uses: silverstripe/gha-keepalive@v1
```
