# Cron

A tiny amendment to tmeasday's very useful meteor-cron package.  I experienced a problem in which cron timing counters were exceeding their own targets (i.e. a counter reaches 16 even though it should run every 15 minutes), at which point the job will never run again without a server restart as the original meteor-cron only tests for equality.  This fork corrects that behaviour.

## Use

Adds a simple cron interface to meteor. I'm fairly sure there is a better way to do this, but this works.

```js
  var MyCron = new Cron();
  
  // this job will happen every 1 second
  MyCron.addJob(1, function() {
    console.log('tick');
  });
```

You can also schedule jobs trigger based on timing. You should note that if the time that you schedule happens before a tick it will get delayed until the tick happens.

```js
  var MyCron = new Cron(1000);

  //Get the current unix time in seconds
  var ts = Math.round((new Date()).getTime() / 1000);

  //This job will get called once after 5 second
  MyCron.addScheduleJob(ts + 5, function() {
  	console.log('schedule tick');
  });
```

You can also use this trick to create something amazing.

```js
  var MyCron = new Cron(100);

  //Get the current unix time in seconds
  var ts = Math.round((new Date()).getTime() / 1000);

  //The recursive function
  var recur = function() {

  	//Get the current unix time in seconds
  	var ts = Math.round((new Date()).getTime() / 1000);

  	//Getting a random number
  	var ran = Math.round(Math.random()*10)

  	console.log('schedule tick. I will get called again in ' + ran + ' second(s)');

  	//Create a new schedule before the old one gets deleted
  	MyCron.addScheduleJob(ts + ran, recur);
  };

  //Call the recursive function for the first time
  recur();
```

## Install

Use [meteorite](http://oortcloud.github.com/meteorite/):

Then add via:

```bash
mrt add cron-tick
```

