# Renovate rollout

The organization-wide policy lives in [`renovate-config.json`](./renovate-config.json).
The hosted Renovate app automatically uses this preset for KoG-teeworlds repositories
when it is installed for the organization.

## Safe migration order

1. Install the Renovate GitHub App for the selected active repositories.
2. Confirm that Renovate opens its Dependency Dashboard/onboarding pull request and
   resolves the shared organization preset.
3. Merge onboarding pull requests in dependency order, starting with shared libraries
   and APIs before their consumers.
4. Confirm at least one successful Renovate run and that vulnerability alerts are
   enabled for each repository.
5. Only then remove the overlapping `version: 2` update entries from that repository's
   `.github/dependabot.yml`. Keep Dependabot security updates enabled until the Renovate
   replacement is verified.

Repositories that need an explicit onboarding file can use:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>KoG-teeworlds/.github:renovate-config"]
}
```

The shared policy deliberately does not automerge vulnerability, minor, pre-1.0, major,
or lockfile-maintenance updates. Stable patch, pin, and digest updates are automerged by
Renovate only when the branch is current and every required status check succeeds.
