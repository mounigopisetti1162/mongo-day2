# Find all the topics and tasks which are thought in the month of October


db.topics.aggregate([
    {
        $lookup: {
               from: "tasks",
               localField: "topicid",
               foreignField: "topicid",
               as: "taskinfo"
             }
    },
    {
        $match:{$and:[{$or:[{topic_date:{$gt:new Date("30-sep-2021)}},{topic_date:{$lt:new Date("1-nov-2021")}}]},
        
          {$or:[{"taskinfo.due_date":{$gt:new Date("30-sep-2021")}},{"taskinfo.due_date":{$lt:new Date("1-nov-2021")}}]}
        ]}
    }
    
    ])


....................................................................................................................

# Find all the company drives which appeared between 15 oct-2020 and 31-oct-2020


db.comapnydrives.find({$or:[{drive_date:{$gte:new Date("15-oct-2021")}},
{drive_date:{$lte:new Date("31-0ct-2021")}}]})

.....................................................................................................................
# Find all the company drives and students who are appeared for the placement.

db.comapnydrives.aggregate([
    {
        $lookup: {
              from:"users",
              localField:"userid",
              foreignField:"userid",
              as :"userinfo"
             }
    },
    {
        $project:{
            _id:0,
            "userinfo.name":1,
            company:1,
            drive_date:1,
            "userinfo.email":1,
            "userinfo.userid":1
        }
    }
    ])
..................................................................................................................................
# Find the number of problems solved by the user in codekata

db.codekata.aggregate([
    {
        $lookup: {
               from: "users",
               localField: "userid",
               foreignField: "userid",
               as: "userinfo"
             }
    },
    {
        $project:{
            _id:0,userid:1,problems:1,"userinfo.name":1
        }
    }
    ])
   
............................................................................................................................

# Find all the mentors with who has the mentee's count more than 15

db.users.aggregate([
   {$lookup: {
          from: "mentors",
          localField: "mentorid",
          foreignField: "mentorid",
          as: "mentorInfo"
        }},
   {$group:{_id:{mentorid:"$mentorInfo.mentorid",mentorname:"$mentorInfo.mentorname"},mentee_count:{$sum:1}}},
   {$match:{mentee_count:{$gt:15}}},
   
   ])
