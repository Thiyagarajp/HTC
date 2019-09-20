# HTC
package test;

import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.List;
import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import org.testng.annotations.Test;
import com.mysql.jdbc.Connection;
import com.mysql.jdbc.ResultSet;
import com.mysql.jdbc.Statement;

public class DataBaseTesting extends Common_Methods {

	@Test
	public  void CreatePackage() throws ClassNotFoundException, SQLException {
		//Object of Connections. It is used to connection mysql server DB.
		Connection conn = null;

		// Object of Statement. It is used to create a Statement to execute the query
		Statement stmt = null;

		//Object of ResultSet => 'It maintains a cursor that points to the current row in the result set'
		ResultSet rs = null;
		Class.forName("com.mysql.jdbc.Driver");
		// Open a connection
		conn = (Connection) DriverManager.getConnection("jdbc:mysql://localhost:3306/wps", "root", "123Welcome");
		// Execute a query
		stmt = (Statement) conn.createStatement();
		rs = (ResultSet) stmt.executeQuery("select * from wps_users where EMAIL NOT In ('PKG.admin@test.local','bu@test.local');");//'pkg.mani@test.local',
		while(rs.next())
		{
			Browser_lanch();
			type(locators("cssSelector","#Email"), rs.getString("EMAIL"));
			type(locators("cssSelector","#Password"),"Wps@2019");
			click(locators("xpath","//input[@value='LOGIN']"));
			//get the All packages count
			List<WebElement> AllCounts = driver.findElements(By.xpath("//div[@class='cardContent']/p[@class='cardCount']"));
			int s = AllCounts.size();
			//allpack.click();
			for(int i=0;i<s;i++)
			{
				WebElement ele = driver.findElements(By.xpath("//div[@class='cardContent']/p[@class='cardCount']")).get(i);
				String count = ele.getText();
				ele.click();

				int result = Integer.parseInt(count);			
				//printv(result);
				//Verify the Table count Rows
				List<WebElement> allpacklist = driver.findElements(By.xpath("//table[@id='adminTable']//tbody/tr"));
				int size = allpacklist.size();
				if(size==result)
				{
					print("Count is matched: "+size +" = "+result);
				}
			}
			locators("xpath","//tr[4]//img[@src='images/ViewPackage.png']").click();
			String status = locators("xpath","//p/span").getText();
			//print(status);
			if(status.equals("COMPLETED"))
			{
				print("This package has been completed");
			}
			WebElement element = locators("xpath","//div[@class='Logo']/a/img");
			action(element);
			WebElement Elemlog = locators("xpath","//input[@src='/images/Logout.png']");
			action(Elemlog);
			close();
			break;

		}
	}

}
