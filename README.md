# Ray's Blog 🎒

Welcome to my personal blog. This site is continuously evolving as I dedicate time to update and enhance it. You can explore various templates at [Scripts](https://github.com/JustGoodThemes/Scriptor-Jekyll-Theme). Visit [my blog website](https://huruilizhen.github.io) to catch up with my latest posts.

To run local deployment test run following commands:
```bash
docker run --rm \
--volume="$PWD:/srv/jekyll:Z" \
--volume="$PWD/vendor/bundle:/usr/local/bundle:Z" \
-p 4000:4000 \
-it jekyll/jekyll \
jekyll serve --host 0.0.0.0
```
