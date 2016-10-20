CakePHP 2.x Revisionable Behavior
====================================

This plugin adds a revisionable behavior to any of your models by saving a serialized copy of your model's data using the Model::beforeSave() callback.

Setup
-----

1) Install the revisions table into your project using Console/cake schema
	
	Console/cake schema update -v -p Revisionable

2) Add the behavior to your model

	public $actAs = array('Revisionable.Revisionable');

3) CakePHP's schema create script doesn't account for longblob. After you run `Console/cake schema update -v -p Revisionable` run the included MySQL patch file: `Config/Schema/revisions_table-post_create.patch.sql`

Configuration
----------------------

If you already have a table named revisions in your project you can change up the Config/Schema/schema.php file table name and pass in a `$Model->alias` using the revisionableModel configuration key through the `$actAs` variable. For example, lets call the revisions table `rebitchins`

	public $actAs = array(
		'Revisionable.Revisionable' => array(
			'revisionableModel'=>'Rebitchins'
		 )
	);


Usage
-----

Call the behavior in your controller. Here is an example for `Models/PagesModel.php` in `Controllers/PagesController.php`:

	$revisions = $this->Page->listRevisions($this->Page->id);

	print_r($revisions);
	array(
		'2011-12-05 01:01:01'=>array(
			'Pages' => array(
				'id' => '4edd8cc3-a628-490b-8cbf-6435ac1005e9',
				'name' => 'foobar',
				'body' => '....'
			)
		)
	);

	$row_id = '4edd8cc3-a628-490b-8cbf-6435ac1005e9'; 
	$date = '2011-12-05 01:01:01';
	$this->Page->restoreVersionByDate($this->Page,$row_id,$date);

TODO:
-----

* **UI**: Add a diff viewer
* **Convenience**: Add methods allowing:
	1. 	Show first revision
	2. List one field for all revisions of a model, like body.
	3. See https://github.com/Hyra/revision for hints.
* **API Usability**: Allow calling methods without passing in the Model. For example restoreVersionByDate("2011-12-05 01:01:01") needing a $row_id or $Model.
* **Increase Eficency**: Dynamically install database tables for each Revisionable Model; duplicate Model's tables on first Model::beforeSave(). Tables would be names something like revisionable__pages. Model->data is inserted on each Model::beforeSave(). This allows for selection instead of having to hunt through deserialized data.

