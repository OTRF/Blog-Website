# Open Threat Research Blog

[![Open_Threat_Research Community](https://img.shields.io/badge/Open_Threat_Research-Community-brightgreen.svg)](https://twitter.com/OTR_Community)
[![Open Source Love](https://badges.frapsoft.com/os/v3/open-source.svg?v=103)](https://github.com/ellerbrock/open-source-badges/)

 Sharing and collaborating to empower the Infosec community!

 ## Site: [https://blog.openthreatresearch.com/](https://blog.openthreatresearch.com/)

 ## Contributing

1. Install Jekyll
2. Create markdown files on docs/_posts
3. Create tags or author profiles in docs/_data
4. In `docs/` run `JEKYLL_ENV=production bundle exec jekyll serve` to deploy site locally
5. Browse to `http://127.0.0.1:4000//` and verify your blog post looks the way you want it
6. Create pull request

## Pushing updates to site (Administrators ONLY)

Push site to github pages with the following command 

1. Update local repository after PR is merged `git update`
2. In `docs/` run `JEKYLL_ENV=production bundle exec jekyll serve` to deploy site locally and generate files in `docs/_site`
3. Browse to `http://127.0.0.1:4000//` and verify everything looks good
4. Go back to the root directory of the repo and run `ghp-import -p -c blog.openthreatresearch.com -f docs/_site`