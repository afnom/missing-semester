# The Missing Semester of Your CS Education (UoB Edition)

[![pages-build-deployment](https://github.com/afnom/missing-semester/actions/workflows/pages/pages-build-deployment/badge.svg)](https://github.com/afnom/missing-semester/actions/workflows/pages/pages-build-deployment)

A fork of [MIT's Missing Semester](https://github.com/missing-semester/missing-semester) Coursework.

As with the original, contributions are most welcome though might be more useful if submitted against MIT's repository.  If you have edits or new content to add, please open an issue or submit a pull request.

## Development

To build and view the site locally, run:

```bash
bundle exec jekyll serve -w
```

If you'd prefer to develop the site in a Docker container (e.g., to avoid having to install Ruby and dependencies on your host machine), run:

```bash
docker-compose up --build
```

Then, navigate to <http://localhost:4000> on your host machine to view the website. Jekyll will re-build the website as you make changes to files.

## License

All the contents in this course, including the website source code, lecture notes, exercises, and lecture videos are licensed under Attribution-NonCommercial-ShareAlike 4.0 International [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). See [here](https://missing.csail.mit.edu/license) for more information on contributions or translations.
