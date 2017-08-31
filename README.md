##Replication Package for the paper "Measuring the cost of regression testing in practice: A study of Java projects using continuous integration."

The paper has been accepted at [FSE 2017](http://esec-fse17.uni-paderborn.de/). 

* [DOI](https://doi.org/10.1145/3106237.3106288)
* [PDF](https://www.cs.ubc.ca/~rtholmes/papers/fse_2017_labuschange.pdf)

BibTex:

```
@inproceedings{fse_2017_labuschagne,
 author = {Adriaan Labuschagne and Laura Inozemtseva and Reid Holmes},
 title = {Measuring the Cost of Regression Testing in Practice: {A} Study of Java Projects Using Continuous Integration},
 booktitle = {Proceedings of the Joint Meeting on Foundations of Software Engineering (ESEC/FSE)},
 year = {2017},
 pages = {821--830},
 url = {http://doi.acm.org/10.1145/3106237.3106288},
 doi = {10.1145/3106237.3106288}
} 
```

The data analyzed for the paper can be found in this repository. The data can be used for a variety of studies, e.g., investigating test suite evolution, characteristics of failing tests, or the use of continuous integration in practice.

Travis build logs and the Git repositories themselves are not provided because of their size. They can, however, be downloaded form Travis CI and Github.

The data is provided as a [MySQL dump](db.sql.zip) and contains the following tables:


###repositories

| Field                   | Type         | Description |
|-------------------------|--------------|--------------|
| id                      | int(11)      | id as found on Travis|
| slug                    | varchar(255) | The slug (e.g., apache/pdfbox) that identifies the project on Github and Travis|
| last\_build_id          | int(11)      | The last build run at time of download|
| last\_build_number      | int(11)      | Number of last build run at time of download|
| last\_build_state       | varchar(255) | State of the last build at time of download|
| github_language         | varchar(255) | Language of repository as stored on Travis|
| github\_language_github | varchar(255) | Language of repository as stored on Github (not necessarily the same as on Travis)|
| created_at              | date         | Date created on Github|


###builds

| Field                | Type         | Description |
|----------------------|--------------|-------------|
| id                   | int(11)      | id as found on Travis |
| repository_id        | int(11)      | Repository the build belongs to|
| commit_id            | int(11)      | Last commit in the build|
| number               | int(11)      | Build number that starts at zero for each repository|
| pull_request         | tinyint(1)   | Wether or not the build is associated with a pull request|
| pull\_request_title  | varchar(255) | Title of pull request |
| pull\_request_number | int(11)      | Pull request number as found on Github|
| config               | text         | Build configuration|
| state                | varchar(255) | Passed, failed, errored, started, or canceled|
| started_at           | datetime     | Start time of build|
| finished_at          | datetime     | Finish time of build|
| duration             | int(11)      | How long the build took to execute|
| job_ids              | text         | Jobs associated with the build|
| fix_type             | varchar(255) | Type of changed that fixed the build (if it is apart of a tuple, see paper for details)|


###commits

| Field           | Type         | Description |
|-----------------|--------------|-------------|
| id              | int(11)      | id as found on Travis|
| sha             | varchar(255) | SHA of commit|
| branch          | varchar(255) | Branch the commit is on|
| message         | text         | Commit message|
| committed_at    | datetime     | - |
| author_name     | varchar(255) | - |
| author_email    | varchar(255) | - |
| committer_name  | varchar(255) | - |
| committer_email | varchar(255) | - |
| compare_url     | varchar(255) | Github compare URL|
| in_repo         | tinyint(1)   | Whether the commit is in the repository or not|


###tests

Contains tests that failed in the studied tuples of 40 projects.

| Field          | Type         | Description |
|----------------|--------------|-------------|
| id             | int(11)      | Auto increment id|
| job_id         | int(11)      | Job the test belongs to|
| name           | text         | Test identifier|
| fix_type       | varchar(255) | Code change only (c), Test change only (t), Test and code changes (ct)|

There are also three tables containing data for the re-executed builds. The `rerun_builds` table is the same as the `builds` table, except that it has a `build_id` column referencing the original build. The `rerun_jobs` and `rerun_commits` tables have the same schema as their original counterparts. Please see the paper for more information on how the re-executions were performed.





