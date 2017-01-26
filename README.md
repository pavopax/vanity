# What

You can now get PyPi download counts for multiple packages, outputted into a single file, `results.txt`

Sample Usage:

	python vanity.py numpy              # one
	python vanity.py numpy pandas       # multiple
	python vanity.py numpy pandas -q	# quiet, return only total


# Extract counts from results.txt

You may use code like this to get a pandas dataframe of the counts, given the
`results.txt` file from above

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
    with open("../data/python_pypi_counts.log", "r") as f:
        lines = [line.strip() for line in f.readlines() if line.strip()]
    return [extract_count(line) for line in lines
            if extract_count(line) is not None]

counts = get_pypi_counts()

pypi = pd.DataFrame(counts, columns=['package', 'downloads'])
pypi.head()
```


# Details

See [this commit](https://github.com/pavopax/vanity/commit/316cd549b1af4d8980f5cd4fc6e7f2851bb83b74)

vanity.py sends output with `logging` commands so sending output with `>` in
command line didn't seem to work.
