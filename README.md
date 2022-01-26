# renovate-npm-bug

### Steps to reproduce
* Create npm package repo
* Configure engines in `package.json` file:
```
{
    "name": "test",
    "version": "1.0.0",
    "description": "test",
    "private": true,
    "engines": {
        "npm": "^8.1.0"
    },
    "dependencies": {
        "big.js": "6.1.0",
        "numbro": "2.0.6"
    }
}
```
* Set Renovate repo config:
```
{
    "extends": [
        "config:base",
        ":preserveSemverRanges",
        ":rebaseStalePrs",
        "group:recommended"
    ],
    "ignorePresets": [
        ":prHourlyLimit2"
    ],
    "bbUseDefaultReviewers": false,
    "enabledManagers": [
        "maven",
        "npm",
        "regex"
    ],
    "packageRules": [
        {
            "matchManagers": [
                "maven"
            ],
            "extends": [
                ":reviewer(user1)"
            ]
        },
        {
            "matchManagers": [
                "npm"
            ],
            "extends": [
                ":reviewer(user2)"
            ],
            "recreateClosed": true,
            "minor": {
                "enabled": false
            },
            "major": {
                "enabled": false
            },
            "lockFileMaintenance": {
                "enabled": true
            },
            "ignoreDeps": [
                "numbro"
            ]
        }
    ]
}
```
* Set `"binarySource": "install",` in Renovate config:
```
{
  "platform": "github",
  "gitAuthor": "Renovate Bot <petrovich@example.com>",
  "repositories": [
    "okgolove/renovate-npm-bug"
  ],
  "binarySource": "install"
}

```
* Run renovate

### Expected Result
Latest version of npm is set up, dependencies have been updated successfully, a PR has been created


### Actual Result
Properly npm version is installed, but Renovate fails to update dependencies (logs are attached)

