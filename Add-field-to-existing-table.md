# 1. Create a migrate file

    php artisan make:migration add_new_field_to_tablename
    
# 2. Create functions up and down
You need to add 2 new functions for the up and down grading of your table

```
public function up()
{
    Schema::table('tablename', function($table) {
        $table->integer('name_of_field');
    });
}
```

```
public function down()
{
    Schema::table('tablename', function($table) {
        $table->dropColumn('name_of_field');
    });
}
```
