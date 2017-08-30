# Workflow

## For a developer creating a new template
- Admin creates a fork of [template-starter](https://github.com/ubiweb-media/template-starter) with the name: `template-{name}`
- Create a corresponding composer [Ubiweb package](https://packagist.org/packages/submit) on packagist.org. Connect this to its git repo so it get's automatically updated with every update.
- Provide the new repo to a dev and have them follow the template usage instructions
- Repo can be read only. Have the dev fork it and they can create a pull request for the admin to accept. This is best used for devs that are not part of the organization so you don't need to grant Github account access. Otherwise, if for example you're working with an internal dev, simply give write access to this repo and they can push their commits directly.

When a new template is done. You should also consider doing a `composer require ubiweb/template` on a specially set up domain, like https://templates.ubiweb.ca, so you have a catalog of templates. Also add to the names of templates on the `template-starter` README.

## For a developer creating a new ubiweb domain.
- Admin creates a new repository with the client's domain as the name `domain.com`. 
- Use the **import** option from github to import the code from: [ubiweb-domain](https://github.com/ubiweb-media/ubiweb-domain.git)
- Provide the new repo to your internal developer, who should have full read/write access. They can go through the setup instructions if it's their first time, or simply clone the domain to their domains directory and run their PHP server.
- The `master` branch is the branch that is live on the client's domain. So careful QA should be given to it. 
- The `dev` branch is the main branch used for development. Changes or features should be created as their own branch and merged into this branch. Development is never done directly on this branch.
- Instead you should branch off of `dev` and create a branch with the following naming conventions:
  - `v0`, this is the initial branch used when creating the first version.
  - `v{#}`, this is the version number. Perhaps you plan on a large collection of changes for a new version of the site, like v2. Use this name to denote that. However, you should also branch off of this using the same conventions below if need be.
  - `feature/name-of-the-feature`, any new features.
  - `bugfix/name-of-the-bug`, for bug fixes.
- When a version of is ready for QA, you should first `git merge dev` into the branch you're about to test (to make sure it's current). Then deploy the branch to stage. If it passes, merge it into the `dev` branch. 
- When you're ready to deploy to live, `git merge dev` into your master branch and deploy master.

## Dev Setup
- Set up [ubiweb core and domain](https://github.com/ubiweb-media/ubiweb-domain)
- Install dependicies with `composer install`
- `cd domain.com` and run PHP server with: `php -S localhost:9000`
- Preview your site on [localhost:9000](http://localhost:9000)

## Deployment

### Staging
- SSH into the staging server at 165.227.64.23 (`ssh root@165.227.64.23` - your key needs to be installed first.)
- `cd /var/www/stage.ubiweb.ca && git clone https://github.com/ubiweb-media/domain.com`
- `cd domain.com && composer install`
- Adjust `.env` with the following:
  ```
  CORE_PATH=/var/www/ubiweb/ubiweb-core
  SUBDIR=domain.com
  ```
- Run `git pull origin master` whenever you need to update stage.

The domain will be available at https://stage.ubiweb.ca/domain.com/

### Production

- Create an add-on domain in [Plesk](https://67.207.89.241:8443) under the `ubiweb.ca` subscription.
- SSH into the server and adjust `.env` and change `CORE_PATH=/var/www/httpdocs/ubiweb-core`
- `git pull origin master` in the under the domain to deploy any updates.
- Point `domain.com` nameservers to `nash.ns.cloudflare.com` & `rose.ns.cloudflare.com`

## SEO

[SEO guidelines and checklist](SEO.md). 
