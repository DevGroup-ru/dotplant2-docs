# Background jobs

To automatically start the job, you must register the following command cron  `* * * * * path/to/dotplant/application/yii background/tasks`

To use you must inherit from the controller `yii\console\Controller` and register the task in the Task Manager `/background/manage/index` or use an assistant `app\backgroundtasks\helpers\BackgroundTasks::addTask($params)` if the task is used by event.

Options available for an assistant:

* Name - task name
* Description - job description
* Action - route-to-action
* Options - Options action separated by a space in the order of their performance at work
* Condition - condition for the implementation in the cron format