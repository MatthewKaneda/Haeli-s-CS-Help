## Haeli-s-CS-Help

Scripts and commands that might be necessary

## Helpful git/Github commands and processes

#### Prerequisites
1. **MAKE SURE** you are on the right branch (generally your branch that you want to make edits to)

#### Process
This is starting from the point of if you had just powered on your laptop
1. Open your terminal and cd into your project root directory
2. Ensure you are on your branch. For example if your branch is `haeli` then you will run:

	`$ git checkout haeli`
	- If you are already on branch `haeli` then it will say `Already on 'haeli'`

3. Make your edits and you can take as long as you want with your edits
	- You are currently editing your local branch **NOT** the remote branch

4. To push the edits from your local branch to the remote branch:
	1. First pull from master by doing: `git pull origin master`
		- You may encounter 2 cases:
		1. You may or may not be prompted to add a message for why you want to merge branches
			- If this is the case then you **NEED** to add a message or else the merge will be cancelled.
			- To add a message:
				- Press the `i` key to tell the text editor that you want to "insert" and then use arrow keys to navigate your way through the text.
				- Add some sort of message saying why/what you're merging so something like `merging from master to haeli`.
				- Once you're done typing your message then press the `Esc` key to exit "insert" mode then type `:wq` and press enter.
				- `:wq` saves your changes to that text document and quits/exits out of the document.
				- Your merge should now be complete as long as there were no merge conflicts.
		2. In the even that you get a message that `error: Your local changes to the following files would be overwritten...`
			- use `git stash` to stash your changes
			- `git pull origin master` to pull
			- `git stash apply` to reapply your stashed changes
			- Then you can continue

	2. `git status` to view your changed files and the files available to add to your commit
	3. `git add {filename}` to add those files to be commited aka staging these files to be committed
		- Replace `{filename}` with name of file to add
		- I always like doing a `git status` before committing to be able to see what is added so far
	4. `git commit -m "this is your commit message"` to commit the files you added (staged) to your branch
		- What `git commit` does is that it takes a snapshot of the project's currently staged changes
	5. To finish it all up, you will finally push the commit you just made:
		- `git push origin {name_of_branch}`
			- Replace `{name_of_branch}` with the name of your branch so something like `git push origin haeli` for example.

If anyone pushes to the master branch during any time then be sure to pull from master:

	$ git pull origin master
**NOTE:** even if you pull from master to your local `haeli` branch that does not mean that you pulled to your local `master` branch
	- Be sure to `git checkout master` then `git pull` to sync your local `master` branch with the remote `master` branch

A quick way to update every branch you have:

	$ git pull --all
Seldomly use this as it may not be the best practice since it will pull for every branch and there is little control over it


## Project 6

#### Prerequisites
1. Be sure to be in the project directory (ie Desktop/cs142/project6react)

#### Starting mongodb server locally

While being in project directory, note which folder is your mongodb folder/file (ie `mongo_file`) and run:

	$ mongod --dbpath mongo_file/
**Note** the 2 dashes "--"

If you get an error with something like:

	$ Failed to set up listener: SocketException: Address already in use
**Note** error will be somewhere in the middle of the terminal output. You also know it failed if the terminal exits the task
To fix, you either need to setup a listener or you can run mongod on a different port

#### Option 1 - setup a listener for TCP protocol

	$ sudo lsof -iTCP -sTCP:LISTEN -n -P
	$ Password: 
When running the listener command, you should see the PID of mongod in the list that is printed to the terminal.
If you don't see the PID then you can find it running this command:

	$ ps -ax | grep mongod
**Note** you only have to run this command if you were not able to find the PID in the output of setting up the listener

Kill the running instance of mongod using it's PID

	$ sudo kill -9 <PID of mongod>

Try starting mongo server again:

	$ mongod --dbpath mongo_file/
If the terminal outputs a bunch of text and does not end it's own process then that means that the server successfully started.

#### Option 2 - run mongod on a different port
**Note** this option has not been tested so there is no certainty it will work

Run mongod with `--port` parameter

	$ mongod --dbpath mongo_file/ --port 21000
**Note** replace `mongo_file/` with whichever directory you make
**Note** you need to specify a port so replace `21000` with whatever port you want as long as it is `>= 1000`
If the terminal outputs a bunch of text and does not end it's own process then that means that the server successfully started.

#### Test the server

Running `mongo` to see if you can connect to the server will help you test if you're running the server

If still running on default port (ie you went with **option 1**)

	$ mongo
This should open up a mongo shell where you can view the databases

If you changed the port by going with **option 2** then you have to specify which port

	$ mongo --port 21000

#### Some useful mongo shell commands to view your mongo data

You should either already be in the mongo shell if you followed the troubleshooting thus far.
**Only** if you aren't already in the mongo shell (you don't see `>` starting each line):

	$ mongo
or

	$ mongo --port 21000

To view all databases from your mongo directory:

	> show dbs
	admin          0.000GB
	config         0.000GB
	cs142project6  0.000GB
	local          0.000GB
	>

To select the database you want:

	> use cs142project6
	switched to db cs142project6
	>

To show your different schemas/models:

	> show collections
	photos
	schemainfos
	users
	>

To view data from a specific collection:

	> db.schemainfos.find().pretty()
	{
		"_id" : ObjectId("6036aca461d119372afb0d01"),
		"version" : "1.0",
		"load_date_time" : ISODate("2021-02-24T19:44:36.554Z"),
		"__v" : 0
	}
	>
The `.find()` attribute is the same as when using it in node.js/express.js where you can put in parameters such as:

	> db.users.find({ 'first_name': 'Ian' }).pretty()
	{
		"_id" : ObjectId("6036aca361d119372afb0ce2"),
		"first_name" : "Ian",
		"last_name" : "Malcolm",
		"location" : "Austin, TX",
		"description" : "Should've stayed in the car.",
		"occupation" : "Mathematician",
		"__v" : 0
	}
	>




