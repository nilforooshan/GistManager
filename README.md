# Gist Manager

### Create a JSON file of all gists

Set your password.

```sh
PASSWD=""
```

Then,

```sh
rm -f gists.json
for i in {1..5} # Increase if you got more than 250 public gists.
do
   curl --user "nilforooshan:$PASSWD" -i "https://api.github.com/users/nilforooshan/gists?&page=$i&per_page=50" >> gists.json
done
```

### Create `public_gists.md` and `private_gists.md` from `gists.json`.

```sh
grep html_url    gists.json | sed -e 's/\"//g' -e 's/,//' -e 's/ //g' -e 's/html_url://' | grep -v nilforooshan > url
grep description gists.json | sed -e 's/\"//g' -e 's/,//' -e 's/;//' -e 's/    description: //' > description
grep public gists.json | sed -e 's/\"//g' -e 's/,//' -e 's/ //g' -e 's/public://' > public
paste -d';' description url | sed 's/;/](/' | sed 's/^/[/' | sed 's/$/)/' > tmp.md
paste -d'_' tmp.md public > gists.md
grep _true gists.md | sed 's/_true//' > public_gists.md
grep _false gists.md | sed 's/_false//' > private_gists.md
rm description url public tmp.md gists.md
```

### Extract public gists from a specific tag

```sh
tag=(R sh sed awk py f90 md git aws)
grep "${tag[0]}:" public_gists.md
```

### Extract private gists from a specific tag

```sh
tag=(R sh sed awk py f90 md git aws workflow)
grep "${tag[0]}:" private_gists.md
```
