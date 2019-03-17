# Gist Manager

**Only for public gists**

### Create a JSON file of all gists

```sh
rm -f gists.json
for i in {1..3} # Increase if you got more than 150 public gists.
do
   curl -i "https://api.github.com/users/nilforooshan/gists?&page=$i&per_page=50" >> gists.json
done
```

### Create a Markdown file from the JSON file

```sh
grep html_url    gists.json | sed -e 's/\"//g' -e 's/,//' -e 's/ //g' -e 's/html_url://' | grep -v nilforooshan > url
grep description gists.json | sed -e 's/\"//g' -e 's/,//' -e 's/;//' -e 's/    description: //' > description
paste -d';' description url | sed 's/;/](/' | sed 's/^/[/' | sed 's/$/)/' > gists.md
rm description url
```

### Extract gists from a specific tag

```sh
tag=(R sh sed awk py f90 md git aws)
grep "${tag[0]}:" gists.md
```
