=================
MkDocs Deployment
=================

Build MkDocs documentation and transfer build files to a target branch.
The target branch contents can be copied to any static site hosting platform such as GitHub Pages.

Usage
=====

This action only helps you build and commit MkDocs documentation to ``target_branch``,
branch. 

Also we need to use some other actions:

- ``action/setup-python@v5`` for installing Python and Pip
- ``actions/checkout`` for checking out git repository
- ``ad-m/github-push-action`` for pushing static build files to remote target branch

For example, you can create a YAML file for your GitHub workflow with the following contents 
and save it under the ``.github/workflows`` directory in your repository:

.. code-block:: yaml

   name: MkDocs Deploy
   on:
     push:
       branches:
       - main
   jobs:
     build:
       runs-on: ubuntu-latest
       steps:
       - uses: actions/setup-python@v5
         with:
           python-version: 3.12.6
       - uses: actions/checkout@v4
         with:
           fetch-depth: 0 # otherwise, you will failed to push refs to dest repo
       - name: Build and Commit
         uses: iSOLveIT/mkdocs-notes@main
         with:
           config_file_path: mkdocs.yml 
       - name: Push changes
         uses: ad-m/github-push-action@master
         with:
           github_token: ${{ secrets.GITHUB_TOKEN }}
           branch: site

Inputs
======

======================= ================ ============ ===============================
Input                   Default          Required     Description
----------------------- ---------------- ------------ -------------------------------

``config_file_path``    ``'mkdocs.yml'`` ``true``     Relative path to the 
                                                      `mkdocs.yml` configuration 
                                                      file
``documentation_path``  ``'./docs'``     ``false``    Relative path under
                                                      repository to documentation
                                                      source files
``target_branch``       ``'site'``       ``false``    Git branch where assets will
                                                      be deployed
``target_path``          ``'.'``         ``false``    Directory in ``$target_branch``
                                                      where MkDocs Pages will be
                                                      placed
``repository_path``     ``'.'``          ``false``    Relative path under
                                                      ``$GITHUB_WORKSPACE`` to
                                                      place the repository.
                                                      You not need to set this
                                                      Input unless you checkout
                                                      the repository to a custom
                                                      path
``requirements_path``   ``'.'``          ``false``    Relative path under
                                                      ``$repository_path`` to pip
                                                      requirements file
``mkdocs_version``      ``'1.4'``        ``false``    Custom version of MkDocs
======================= ================ ============ ===============================
