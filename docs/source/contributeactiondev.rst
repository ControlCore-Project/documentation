The Concore Action Dev docs
===========================

- On triggering ``contribute`` action in fri server, it executes the ``contribute.py`` script

- Let's assume the paramters provided are:
    - Study Name - test study
    - Study Path - F:\\example
    - Author Name - test user

- It will create a new branch named ``AuthorName_StudyName`` i.e test_user_test_study in the bot repository ``parteekcoder/concore`` which references the default branch in ``parteekcoder123/concore``

.. image:: images/ca-github-branch-ref.png
  :width: 700

- Now creating Github blob (refer https://docs.github.com/en/rest/git/blobs ) for each file in the directory here ``F:\\example``. After creating blobs , create a new Git tree (refer https://docs.github.com/en/rest/git/trees)

- After creating Github tree, change the refrence of the newly created branch to the that tree

.. image:: images/ca-github-working-chart.png
  :width: 700

- Now the changes have been pushed to github, and for creating pull request the  ``pull_request.yml`` workflow is runned which creates pull request to upstream repo using persoanl access token of upstream repo

``Note: You need to store the personal access token of upstream repo as github secret in bot repo``

https://dev.to/bro3886/create-a-folder-and-push-multiple-files-under-a-single-commit-through-github-api-23kc