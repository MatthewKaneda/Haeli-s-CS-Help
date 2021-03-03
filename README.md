## Haeli-s-CS-Help

Scripts and commands that might be necessary for each project

## Project 6

#### Prerequisites
1. Be sure to be in the project directory (ie Desktop/cs142/project6react)

#### Starting mongodb server locally

While being in project directory, note which folder is your mongodb folder/file (ie `mongo_file`) and run:

	$ mongod --dbpath mongo_file/
**Note** the 2 dashes "--"

If you get an error with something like:

	$ Failed to set up listener: SocketException: Address already in use
Then you either need to setup a listener or you can run mongod on a different port

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






