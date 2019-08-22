# April 17th, 2019

## debugging elasticsearch

tags: `#elasticsearch #homebrew`

To install elastic search locally on a mac:

1. install java8
2. run `brew install elasticsearch`
3. run `brew services start elasticsearch`

If you run into any errors connecting to elastic search:

- run `brew ls elasticsearch`
- find a file that looks like `/usr/local/{version}/bin/elasticsearch`
- run that file to see errors

In my case I had a directory misnamed. By renaming it to the path it was looking for fixed my issue.
