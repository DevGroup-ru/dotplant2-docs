# Background tasks

To automatically run the tasks necessary to register in the cron following `* * * * * path/to/asgard/application/yii background/tasks`

To use you must inherit `yii\console\Controller` and register the action in the task manager using the interface `/background/manage/index` or use the helper `app\backgroundtasks\helpers\BackgroundTasks::addTask($params)` if the task arises by event.

Available options for the helper:

* name - the name of the task
* description - the description of the task
* action - the route to action
* params - parameter string through the gap in the order they appear in the action movie
* init_event - initiating event
