+++

title = "Using GitHub Issues for Blog Comments"
slug = "using-github-issues-for-blog-comments"
date = 2020-03-22
[taxonomies]
tags = ["GitHub", "static-website", "blog", "disqus"]
+++

In the past, [disqus](https://disqus.com/) was the solution to go for when you needed a functionality for your static website that would enable people to post comments and ask questions under your posts.  
With it's free tier, you would get a package with basic features. It comes with a price though of it being relatively heavy solution.  
It's ads supported and increases page weight by factor of `10`! Not to mention it lets other applications like Facebook to track your visitors.  
So recently when I was in middle of integration of `disqus` plugin to this blog, I decided to revisit my product of choice and explore other options in nowadays product driven market.  

<!-- more -->
*Scroll down for **DLDR;***

It did not take long to find few very interesting alternatives.  
For example [Talkyard](https://www.talkyard.io/) and [Commento](https://commento.io/) are open-source projects with focus on privacy and come with all major features. You can host them eg.: on `heroku` for free or pay reasonable price for product as a service.  
Although they are both robust solutions which I do not need for my use case.  


However [Staticman](https://github.com/eduardoboucas/staticman) was the project that cough my eye:  
> Staticman is a Node.js application that receives user-generated content and uploads it as data files to a GitHub and/or GitLab repository. In practice, this allows you to have dynamic content (e.g. blog post comments) as part of a fully static website, as long as your site automatically deploys on every push to GitHub and/or GitLab.

It'd still require me to host my own server though.  
At this point I got an idea - since I host this blog on [GitHub](https://github.com/fogine/blog), I could create a comment system which would integrate with `GitHub Issues`.  
When somebody comments on my blog I'd use `GitHub API` to create an issue and store the discussion there.  
For my target audience this would be ideal solution!  
It turned out that there is already a `GitHub widget` for that. It's called [utterances](https://github.com/utterance/utterances).  

## TLDR;
________
### Here is how it works:  

- You as a blog owner install [Utterances GitHub App](https://github.com/apps/utterances) on the GitHub repository that hosts comments.
- You as a blog owner include simple script where you want the comments to load:
```javascript
<script src="https://utteranc.es/client.js"
        repo="fogine/blog"
        issue-term="pathname"
        label="Comment"
        theme="github-dark-orange"
        crossorigin="anonymous"
        async>
</script>
```
- The script uses the GitHub [issue search API](https://developer.github.com/v3/search/#search-issues) to find the issue associated with the page based on post's `pathname`. 
- If an issue does not exist, [utterances-bot](https://github.com/utterances-bot) will create it first time someone comments.
- To comment, users must authorize the `utterances` app to create issues (for the blog repository only) on their behalf.
- Alternatively, users can comment on the GitHub issue directly.

### Considering limitations of using GitHub Issue tracker service to power blog comments:  
- It's features are basic. For example, you can't reply to someone's comment (You can quote him though).
- GitHub's Search API is `rate limited` to `5000` requests/hour. Which is a little really. So if (I mean When) this blog gets popular enough, I'll have to come with solution that actually scales.


You can see it in action, bellow ⌄⌄⌄
