CakePHP 2.x Revisionable Behavior
====================================

This is a simple small plugin that adds a revisionable behavior to any of your models

It does this by grabbing a copy of your row right before you save it and saving a serialized copy of it in the serialize table

This plugin is pretty easy to use

Setup
-----

1) First you need to add the table to your project  use the following to generate that
	Console/cake schema update -v -p Revisionable

2) add the behavior to your model like so 

	public $actAs = array('Revisionable.Revisionable');

3)  I've discovered that cakephp has some bugs in it's schema create script that doesn't account for longblob, so make sure that after you run `Console/cake schema update -v -p Revisionable` you go change the data type for the data column to longblob otherwise you may not get your revision saved properly.

Configuration
----------------------

If you already have a table named revisions in your project and need to call the it something else then 
make a new table/model in your project that has the same schema as this plugin and pass that model name
to the actAs setup like so

	var $actAs('Revisionable.Revisionable'=>array('revisionableModel'=>$newModelName));


To Do
-----

So far aside from creating a revision everytime the data changes the only other feature of this plugin
is the listRevisions function.  It works like this

	$revisions = $this->Model->listRevisions($this->Model->id);

	print_r($revisions);
	array(
		'2011-12-05 01:01:01'=>array(
			'Pages' => array(
				'id'=> '4edd8cc3-a628-490b-8cbf-6435ac1005e9',
				'name'=> 'foobar',
				'body'=> '....'
			)
		)
	);

Need to create some more functions that add to this, like perhaps a diff viewer

-Join the fun!