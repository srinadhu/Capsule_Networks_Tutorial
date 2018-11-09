This directory contains the testing framework of the Markdown package. Tests
act as an ensurance that changes to the package did not introduce new bugs.

The `test.sh` shell script is used to run the individual tests. To run one or
more tests, execute the `test.sh` script with the pathnames of one or more test
files as the arguments. Test files can be recognized by the `.test` suffix and
reside inside the `testfiles/` directory. The Markdown package needs to be
installed to run the tests.

Each time a commit is made to the Git repository of the project, this test
suite is ran by a continuous integration service. The current status is:
[![CircleCI](https://circleci.com/gh/Witiko/markdown/tree/master.svg)][CircleCI]

 [CircleCI]: https://circleci.com/gh/Witiko/markdown/tree/master (CircleCI)
