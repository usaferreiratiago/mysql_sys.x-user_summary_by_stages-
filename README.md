# mysql_sys.x-user_summary_by_stages-

SELECT 
    IF((`performance_schema`.`events_stages_summary_by_user_by_event_name`.`USER` IS NULL),
        'background',
        `performance_schema`.`events_stages_summary_by_user_by_event_name`.`USER`) AS `user`,
    `performance_schema`.`events_stages_summary_by_user_by_event_name`.`EVENT_NAME` AS `event_name`,
    `performance_schema`.`events_stages_summary_by_user_by_event_name`.`COUNT_STAR` AS `total`,
    `performance_schema`.`events_stages_summary_by_user_by_event_name`.`SUM_TIMER_WAIT` AS `total_latency`,
    `performance_schema`.`events_stages_summary_by_user_by_event_name`.`AVG_TIMER_WAIT` AS `avg_latency`
FROM
    `performance_schema`.`events_stages_summary_by_user_by_event_name`
WHERE
    (`performance_schema`.`events_stages_summary_by_user_by_event_name`.`SUM_TIMER_WAIT` <> 0)
ORDER BY IF((`performance_schema`.`events_stages_summary_by_user_by_event_name`.`USER` IS NULL),
    'background',
    `performance_schema`.`events_stages_summary_by_user_by_event_name`.`USER`) , `performance_schema`.`events_stages_summary_by_user_by_event_name`.`SUM_TIMER_WAIT` DESC
