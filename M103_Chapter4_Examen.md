# Final Exam

## Final: Question 1

**Problem:**

Which of the following are valid command line instructions to start a mongod? You may assume that all specified files already exist.
Attempts Remaining:Incorrect Answer

Check all answers that apply:

* `mongod --logpath /var/log/mongo/mongod.log --dbpath /data/db --fork` :+1:

* `mongod --dbpath /data/db --fork`

* `mongod --log /var/log/mongo/mongod.log --authentication`

* `mongod -f /etc/mongod.conf` :+1:

### See detailed answer

The correct answers are:

* `mongod --logpath /var/log/mongo/mongod.log --dbpath /data/db --fork`
 
* `mongod -f /etc/mongod.conf`

The following choices are incorrect:

* `mongod --dbpath /data/db --fork`

This is incorrect because a --logpath must be specified in order to fork the process.

* `mongod --log /var/log/mongo/mongod.log --authentication`

This is incorrect because both `--log` and `--authentication` are invalid flags - instead, they should say `--logpath` and `--auth`.
