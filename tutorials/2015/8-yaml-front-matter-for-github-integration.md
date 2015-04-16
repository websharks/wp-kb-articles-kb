---
title: YAML Front Matter (for GitHub Integration)
categories: tutorials
tags: github-integration, yaml-front-matter
author: jaswsinc
github-issue: https://github.com/websharks/wp-kb-articles-kb/issues/8
toc-enable: false
---

`---`

```yaml
slug: 'quick-example' # (default value is based on repo file path minus extension)
title: 'A Quick Example KB Article' # (default value is pulled from article body)

categories: 'cat-slug1, cat-slug2, 3, 4' # (slugs or IDs; default is no categories)
tags: 'tag-slug1, tag-slug2, 3, 4' # (slugs or IDs; default is no tags)

author: 'username|ID' # (defaults to a user configured in Dashboard for KB articles)
status: 'draft|pending|pending-via-github|future|publish' # (defaults to pending-via-github)
pubdate: 'strtotime compatible' # (defaults to `now`)

excerpt: 'short description/summary' # (default is empty)

comment-status: 'open|closed' # (default based on blog config)
ping-status: 'open|closed' # (default based on blog config)

github-issue: '1|owner/repo#1|URL leading to an issue'
# NOTE: this can be `github-issue: ` or just `issue:`.

link-images: 'true|false' # (defaults to config. option in the Dashboard)
# NOTE: this enables|disables automatic image links; i.e., click to enlarge.

hids-enable: 'true|false' # (defaults to config. option in the Dashboard)
# NOTE: this enables|disables auto-generated heading ID anchors.

toc-enable: 'true|false' # (defaults to config. option in the Dashboard)
# NOTE: this enables|disables an auto-generated Table of Contents.
```

`---`

When WP KB Articles pulls an article from your repo into WordPress for the first time, it will use the YAML Front Matter you provide at the top of the article, encapsulated by separators `---` `---` on their own line as seen above. Default values (as detailed above) will be used for any details that you do not define explicitly in the YAML Front Matter.

Once the article is in WordPress, default values are no longer applied during any future updates. Any YAML Front Matter that _is_ in the file will always be mirrored back to WordPress, but any YAML Front Matter that is not yet defined will remain as-is on the WordPress side; i.e. fields that are not in the YAML Front Matter can be controlled exclusively through WordPress.

This allows you to use as much or as little YAML Front Matter as you like. It comes down to a decision on your part as to whether you want to maintain metadata associated with an article from within WordPress (e.g. not including much, if any, YAML Front Matter); or you can choose to provide YAML Front Matter when you publish an article, or later during an update, and it will be mirrored to your WordPress installation.

---

## To Help Explain This Further

**Q:** How can I update the title of an article?

**A:** If you did not supply a `title:` in the YAML Front Matter, you can simply log into WordPress and change the title there. If you _did_ define a title in the YAML Front Matter, you should change the title in your YAML Front Matter and wait for an automatic update to occur periodically.

---

**Q:** I added YAML Front Matter, but now I've decided that I want control over the title from within WordPress instead. How can I accomplish this?

**A:** Remove the `title:` from your YAML Front Matter and then log into WordPress and you'll have full control over a custom title from that point forward. Fields in your YAML are mirrored back to WordPress periodically (if they exist). Remove the field from your YAML and you gain control over it from within WordPress.

---

**Q:** If I change the categories/tags in the YAML Front Matter, does my new set of categories/tags replace the old set? Or do they get added and others are preserved?

**A:** What you put into the YAML Front Matter is mirrored back to WordPress. If you change categories/tags in the YAML, any old categories/tags are removed automatically and your new configuration will be applied then.

---

**Q:** If I assign an article to one or more categories/tags that do not yet exist, are those categories/tags created on-the-fly automatically?

**A:** Yes. Any categories and/or tags that you define in slug format will be created automatically if they currently do not exist. This makes it very easy to use YAML Front Matter to categorize and/or tag your articles in various ways.

---

**Q:** What about `pubdate:` in YAML Front Matter? If I set that to `now` and then it updates again later will that change the pubdate each time the article content changes?

**A:** It's for this very reason that a relative `pubdate:` in your YAML will be used only during the initial insertion of the article when it goes into WordPress for the first time. Therefore, a relative `pubdate` is one exception to the rules above.

For instance, if you set `pubdate: now` or `pubdate: +3 days`, those are relative times. They apply only during the initial insertion of the article. Relative dates are bypassed on all future article updates.

However, you can still change the publish date via YAML Front Matter. You just need to use a non-relative date; e.g. `pubdate: 2015-01-30`, `pubdate: Jan 30th, 2015`, etc. Anything compatible with PHP's `strtotime()` function.
