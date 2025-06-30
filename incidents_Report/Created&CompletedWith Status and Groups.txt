SELECT 
    "wo"."WORKORDERID" AS "Request ID",
    "std"."STATUSNAME" AS "Status",
    "mdd"."MODENAME" AS "Request Mode", 
    "ti"."FIRST_NAME" AS "Technician", 
    "cd"."CATEGORYNAME" AS "Category", 
    "cri"."FIRST_NAME" AS "Created By", 
    "dpt"."DEPTNAME" AS "Department", 
    "wotodesc"."FULLDESCRIPTION" AS "Description", 
    "qd"."QUEUENAME" AS "Group", 
    "aau"."FIRST_NAME" AS "Requester", 
    "wo"."TITLE" AS "Subject", 
    "wos"."ASSIGNEDTIME" AS "Assigned Time", 
    "serdef"."NAME" AS "Service Category", 
    "wo"."CREATEDTIME" AS "Created Time",
	"wo"."COMPLETEDTIME" AS "Completed Time"
FROM "WorkOrder" "wo"
LEFT JOIN "ModeDefinition" "mdd" ON "wo"."MODEID" = "mdd"."MODEID"
LEFT JOIN "SDUser" "sdu" ON "wo"."REQUESTERID" = "sdu"."USERID"
LEFT JOIN "AaaUser" "aau" ON "sdu"."USERID" = "aau"."USER_ID"
LEFT JOIN "DepartmentDefinition" "dpt" ON "wo"."DEPTID" = "dpt"."DEPTID"
LEFT JOIN "WorkOrderStates" "wos" ON "wo"."WORKORDERID" = "wos"."WORKORDERID"
LEFT JOIN "CategoryDefinition" "cd" ON "wos"."CATEGORYID" = "cd"."CATEGORYID"
LEFT JOIN "SDUser" "td" ON "wos"."OWNERID" = "td"."USERID"
LEFT JOIN "AaaUser" "ti" ON "td"."USERID" = "ti"."USER_ID"
LEFT JOIN "StatusDefinition" "std" ON "wos"."STATUSID" = "std"."STATUSID"
LEFT JOIN "WorkOrder_Queue" "woq" ON "wo"."WORKORDERID" = "woq"."WORKORDERID"
LEFT JOIN "QueueDefinition" "qd" ON "woq"."QUEUEID" = "qd"."QUEUEID"
LEFT JOIN "SDUser" "crd" ON "wo"."CREATEDBYID" = "crd"."USERID"
LEFT JOIN "AaaUser" "cri" ON "crd"."USERID" = "cri"."USER_ID"
LEFT JOIN "WorkOrderToDescription" "wotodesc" ON "wo"."WORKORDERID" = "wotodesc"."WORKORDERID"
LEFT JOIN "ServiceDefinition" "serdef" ON "wo"."SERVICEID" = "serdef"."SERVICEID"
WHERE 
    (
        (
            "qd"."QUEUENAME" = N'IFS' OR "qd"."QUEUENAME" = N'IFS Group' OR "qd"."QUEUENAME" =N'Information Security' OR "qd"."QUEUENAME"=N'Infrastructure' OR "qd"."QUEUENAME"=N'Infrastructure Group' OR "qd"."QUEUENAME" = N'IT-DB' OR "qd"."QUEUENAME"=N'IT-Development' OR "qd"."QUEUENAME"=N'IT-Support' OR "qd"."QUEUENAME"=N'IT-Systems' OR "qd"."QUEUENAME" = N'IT-Systems Group'OR "qd"."QUEUENAME" = N'IT-TOS'OR "qd"."QUEUENAME" = N'Mobile Service'OR "qd"."QUEUENAME" = N'Pending Accounts'OR "qd"."QUEUENAME" = N'Physical Security'OR "qd"."QUEUENAME" = N'Printing Service'OR "qd"."QUEUENAME" = N'SCCT- IT Staff'
        ) 
        AND 
        (
            (
                "wo"."CREATEDTIME" >= 1735689600000 AND  -- Start of 2024 (2024-01-01 00:00:00)
                "wo"."CREATEDTIME" <= 1830297599000 AND  -- End of 2027 (2027-12-31 23:59:59)
                "wo"."CREATEDTIME" != 0 AND 
                "wo"."CREATEDTIME" IS NOT NULL AND 
                "wo"."CREATEDTIME" != -1
            )
        )
    ) 
    AND "wo"."ISPARENT" = '1';
