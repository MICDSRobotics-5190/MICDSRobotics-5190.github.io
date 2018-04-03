# This is the repository for the Github Pages-hosted website for the FTC Team 5190, Technoramic.

*This is still an experiment to see how good Github pages is, and if we should devote our future efforts here or to some other server.*

Github Pages offers free hosting indefinitely, and ownership is easily transferred as new team members join and old team members graduate through proper roles on the organization. Using a static site generator like Jekyll to set it up means that it is easily maintained if we want to change the domain name, and that it is easy for any member of the team to create a blog post, without forgoing any of the style or customization options we could have with another framework.

## Site Settings

The main settings can be found inside the `_config.yml` file:

- **title:** title of your site
- **description:** description of your site
- **url:** your url
- **paginate:** the amount of posts displayed on homepage per page
- **navigation:** these are the links in the main site navigation
- **social** diverse social media usernames (optional)
- **google_analytics** Google Analytics key (optional)

## Build Setup

1. [Install Jekyll](http://jekyllrb.com)
2. Clone this repository
3. [Install Bundler](http://bundler.io/)
4. Run `bundle install`
5. Install gulp dependencies by running `npm install`
6. Run Jekyll and watch files by running `gulp`
7. Customize and watch the magic happen!

# Alternate Running
Another way of running the local server is either:
- running it as a local host using `npm run server`

or

- building the files locally using `npm run build`