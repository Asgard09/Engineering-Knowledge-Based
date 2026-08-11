# 1. The structure of a Great Commit Message
```
<type>(<scope>): <short description>

[optional body - explain WHAT and WHY, not HOW]

[optional footer(s) - issue tracking, breaking changes]
```
# 2. Standard Commit Types 
|**Type**|**When to Use**|**Example**|
|---|---|---|
|**`feat`**|A new feature for the user|`feat(import): add support for ACB bank Excel format`|
|**`fix`**|A bug fix|`fix(parser): prevent NullPointerException on merged cells`|
|**`refactor`**|Code changes that neither fix a bug nor add a feature|`refactor(service): extract bank parsing logic to strategy classes`|
|**`docs`**|Documentation changes only|`docs(readme): update API endpoint documentation`|
|**`style`**|Code style changes (white-space, formatting, missing semi-colons)|`style(controller): format code according to Google Java style`|
|**`test`**|Adding or correcting tests|`test(parser): add JUnit tests for Vietcombank Excel parser`|
|**`perf`**|Code changes that improve performance|`perf(import): use batch insert for transaction bulk creation`|
|**`chore`**|Updating build tasks, package dependencies, or configs|`chore(deps): upgrade Apache POI to version 5.2.5`|
|**`ci`**|Changes to CI configuration files and scripts|`ci(github): add workflow for automated testing on PR`|
