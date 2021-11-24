# LoadNextNextValuesFromSQL
# Load First 100sql data  Next 100 And So on
# Console App Code
 
using MongoDB.Driver;
using System;
using System.Data.SqlClient;

namespace JsonData
{

    class Program
    {
         static void Main(string[] args)
        {

            using (SqlConnection connection = new SqlConnection("Data Source=10.10.30.20;Initial Catalog=kothainagaraj;Persist Security Info=True;User ID=sa;Password=abc123!@#"))
            {
                connection.Open();
                var minimum = 0;
                var count = 0;
                var condition = 500;
               

                // using (SqlCommand command = new SqlCommand("SELECT  top(@Upper) id,StudentName,age FROM Tab where id >= @Lower ORDER BY id, StudentName, age SET @Lower = @lower + 100 SET @cnt = @cnt + 100", connection)) ;
                // using (SqlCommand command = new SqlCommand(@"DECLARE @count INT;" + "Set @count=0" + @"DECLARE @random INT;" + @"DECLARE @MAX INT;" + "Set @MAX=100" + @"DECLARE @MIN INT;" + "Set @MIN=0" + @"DECLARE @cnt INT;" + "Set @cnt=0" + " while (cnt <= 1000)" + "SELECT  top(@MAX) id,StudentName,age FROM Tab where id >= @MIN ORDER BY id, StudentName, age SET @MIN = @MIN + 100 SET @cnt = @cnt + 100", connection))
                // using (SqlCommand command = new SqlCommand(@"DECLARE @minval INT;" + "set @minval = 0" +  "while (minval <= 1000) "+ "SELECT * FROM Tab ORDER BY id OFFSET @minval ROWS FETCH NEXT 100 ROWS ONLY", connection))
                 while (count<= 1000)
                    {
                    var query = $@"Declare @minval INT, @con INT;
                        set @minval={minimum}                       
                        set @con = { condition }
                        BEGIN
                        SELECT * FROM Tab ORDER BY id OFFSET @minval ROWS FETCH NEXT @con ROWS ONLY
                        
                        END";
                    using (SqlCommand command = new SqlCommand(query, connection))
                    {
                        using (SqlDataReader reader = command.ExecuteReader())
                        {
                            while (reader.Read())
                            {
                                for (int i = 0; i < reader.FieldCount; i++)
                                {
                                    Console.WriteLine(reader.GetValue(i));
                                }
                                Console.WriteLine();

                            }
                            Console.WriteLine(" Data Accessed Successfully");
                            count=count+condition;
                            minimum = minimum + condition;
                            Console.ReadLine();
                        }
                    }
                }
            }
            
        }
    }
}
