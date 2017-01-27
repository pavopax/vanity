# Easy PyPi download counts

You can now get PyPi download counts for multiple packages, outputted into a
single file, `results.txt`, and possibly converted into Pandas dataframe.

In contrast,
the [main repo for `vanity`](https://github.com/aclark4life/vanity) outputs
download counts to terminal, using logging functionality.

Note: You shouldn't necessarily trust the numbers, but they appeared to have
recent data in 1/2017.
See [this issue](https://github.com/aclark4life/vanity/issues/22).

# Sample Usage

Clone this repo, then run:

	python vanity.py ipython				# one
	python vanity.py ipython jupyter		# multiple
	python vanity.py ipython jupyter -q		# quiet, return only total


# Extract counts from results.txt

Runing the above (with `-q` option!), you get a file that includes lines like:

```bash
DEBUG:vanity:ipython - 9,219,641
DEBUG:vanity:jupyter - 398,702
DEBUG:vanity:tensorflow - 35,501
DEBUG:vanity:Theano - 608,210
DEBUG:vanity:No downloads for shogun.
DEBUG:vanity:No such module or package 'pylearn2'
etc
```

If you want a pandas data frame like this...

| index   | package    | downloads |
|---------|------------|---------|
| 0       | ipython    | 9219641 |
| 1       | jupyter    | 398702  |
| 2       | Theano     | 608210  |
| 3       | tensorflow | 35501   |



... then use the following code:


```python
def extract_count(line):
    parts = line.split(" - ")
    if len(parts) == 2:
        package = parts[0].split(":")[-1]
        count = int(parts[1].replace(",", ""))
        return package, count
    else:
        return None

def get_pypi_counts():
    with open("results.txt", "r") as f:
        lines = [line.strip() for line in f.readlines() if line.strip()]
    return [extract_count(line) for line in lines
            if extract_count(line) is not None]

counts = get_pypi_counts()

pypi = pd.DataFrame(counts, columns=['package', 'downloads'])
pypi.head()
```


# Details

See edits to `vanity.py`

vanity.py sends output with `logging` commands so sending output with `>` in
command line didn't seem to work.
