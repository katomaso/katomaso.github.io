# Github Badges

![Coverage Badge](https://badgen.net/badge/you are/awesome/orange?icon=github)

Static badges are easy you either use shields.io or badgen.net services.

There are three approaches
1. storing the report to commit comments
2. storing the report in a `GIST`
3. storing the badge itself in `gh_pages`

## 1. Commit comments

For this one and the later, you will need MishaKav/pytest-coverage-comment action that
extracts pytest-cov report into usable outputs that you can later reference as `${{ steps.coverage.outputs.XY }}`

It can, however, directly post a comment to current commit when user `with: create-new-comment: true`. This
acts as a storage for a webservice, that needs to parse this comment on each call and generate a badge
from it. Such service can be [Samuale's Colvin's Coverage badge](https://github.com/samuelcolvin/coverage-badge)
available on https://coverage-badge.samuelcolvin.workers.dev/OWNER/REPOSITORY.svg . Where you replace the OWNER
and REPOSITORY with your own. This unfortunately works only for PUBLIC repositories. Having you wanted private
generator then you'd need to run this service on your premises with your private github token.

## 2. GIST

GISTs have the advantage of being disconnected from your repository. Hence even private repositories can have
public GISTs without any privacy concerns.

First, you need to generate your pytest-cov report. Beware that **./** is vital. 

```yaml
      - name: Run coverage test and generate badge
        run: poetry run pytest --cov=mavsniff > ./pytest-coverage.txt
```

Then you want to use forementioned action MishaKav/pytest-coverage-comment just to parse the
pytest coverage report to have it available in outputs. 

```yaml
      - name: Extract coverage information
        id: coverage
        uses: MishaKav/pytest-coverage-comment@main
        with:
          pytest-coverage-path: ./pytest-coverage.txt
```

Finally, you want to use [schneegans/dynamic-badges-action](https://github/schneegans/dynamic-badges-action) that
composes a JSON and updates a GIST with it. In order to do that, you need to create a Personal Access token with
only one permission - the "gist permission". Save this token to your repository secrets and use it in your action
as shown bellow.

```
      - name: Create the Badge
        if: github.ref == format('refs/heads/{0}', github.event.repository.default_branch)
        uses: schneegans/dynamic-badges-action@v1.6.0
        with:
          auth: ${{ secrets.GIST_UPDATE_TOKEN }}
          gistID: someLongGistIdObtainedWhenManuallyCreatingThe Gist
          filename: coverage.json
          label: Test coverage
          message: ${{ steps.coverage.outputs.coverage }}
          color: ${{ steps.coverage.outputs.color }}
          namedLogo: python
```

## 3. Github Pages

The action you want is https://github.com/marketplace/actions/coverage-badge that will update image
file in our gh_pages branch. This should work for private repositories since you are GETint the image
with your github cookie directly.
