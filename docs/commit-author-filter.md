# Commit Author Filter Feature

## Overview

The commit author filter feature allows you to filter commits in the history view by username using a special `@{username}` syntax.

## Usage

1. Navigate to the History tab in your repository
2. In the filter text box, type `@` followed by a username (e.g., `@john`, `@mary.smith`, `@dev`)
3. As you type, the commit list will automatically update to show only commits from authors matching the filter
4. The filter uses partial matching, so `@john` will match usernames like `john`, `johnsmith`, `john.doe`, etc.

## Examples

- `@john` - Shows all commits from authors containing "john" in their name or email
- `@mary.smith` - Shows commits from authors matching "mary.smith"
- `@` (just the @ symbol with no username) - Shows all commits (no filtering applied)

## Technical Details

### How It Works

The feature uses Git's built-in `--author` flag which performs a case-insensitive partial match on both the author name and email address. The syntax is:

```bash
git log --author=<pattern>
```

### Implementation

The filter is applied at the data layer when loading commits from the repository. The UI detects when the filter text starts with `@` and extracts the username, then triggers a commit reload with the author filter applied.

### Performance

Filtering is done by Git itself during the log operation, so it's efficient even for large repositories with many commits. The filter is applied both when:
- Loading the initial batch of commits
- Loading additional commits as you scroll through the history

## Related Code

- `app/src/lib/app-state.ts` - State management for author filter
- `app/src/lib/stores/git-store.ts` - Git operations with author filtering
- `app/src/lib/stores/app-store.ts` - Logic for extracting and applying author filter
- `app/src/ui/history/compare.tsx` - UI for triggering author filter
