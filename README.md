# Django ToDo list

This is a to-do list web application with the basic features of most web apps, such as accounts/login, API, and interactive UI. 
To complete this task, you will need:

- CSS | [Skeleton](http://getskeleton.com/)
- JS  | [jQuery](https://jquery.com/)

## Explore

Follow these steps to get the application up and running on your local machine (requires Python 3.8 or higher due to compatibility with Django 4):


```
pip install -r requirements.txt
```

Create a database schema:

```
python manage.py migrate
```

And then start the server (default is <http://localhost:8000>):

```
python manage.py runserver
```

You can now browse the [API](http://localhost:8000/api/) or start on the [landing page](http://localhost:8000/).

## Task

Extend the project's GitHub Actions workflow by integrating Docker to build and push images to DockerHub.
This CI/CD enhancement involves several key tasks:

1. Update your forked repository with your DockerHub username and password.
    1. Add corresponding secrets to the repository.
2. Update `DockerImageName` with the DockerHub image repository name.
3. Add environment secrets for `development` and `staging` environments for your forked repository.
4. Use Matrix to run unit tests on different Python versions (3.8, 3.9).
5. Use Matrix to run unit tests on different OS types: Ubuntu and Windows.
6. You should have the ability to start the workflow manually.
7. Add input variables for the manual workflow start:
    1. Input variables to choose which artifact from the matrix to deploy. (windows-3.8, ubuntu-3.9, etc).
8. Add branch protection to the main branch in your fork.
9. Add mandatory pull requests and `Python CI` job status checks for PRs.
10. Add Manual Approval for the `staging` environment.
11. Allow to run only one workflow per pull request (concurrency).
12. New runs should cancel the previous runs.
13. Create a Pull Request with the changes.
14. Pull Requests description should also contain a reference to a workflow run with successful
workflow execution.
15. Provide screenshots confirming that branch protection and status checks are working as expected.
16. Provide a screenshot confirming `staging` deployment requires manual approval.
