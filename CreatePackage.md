package test;

import java.util.List;
import org.openqa.selenium.WebElement;
import org.testng.annotations.BeforeTest;
import org.testng.annotations.Test;

public class VerifyPackagecounts extends Common_Methods {
	//Pass the Excel file name
	@BeforeTest
	public void setData() {
		excelFileName  = "createpack";
	}
	//Data Provide get the datas from the excel sheet.
	@Test(dataProvider="fetchData")
	public void createPack(String userName, String password)
	{
		startApp_Browser("chrome","http://localhost:3000");
	
	//userName(rs.getString());
	type(locateElement("css","#Email"),userName);
	//type(locators(),userName);
	type(locateElement("css","#Password"),password);
	click(locateElement("xpath","//input[@value='LOGIN']"));
	//get the All packages count
	List<WebElement> AllCounts = locateElements("xpath","//div[@class='cardContent']/p[@class='cardCount']");
	int s = AllCounts.size();
	//allpack.click();
	for(int i=0;i<s;i++)
	{
		WebElement ele = locateElements("xpath","//div[@class='cardContent']/p[@class='cardCount']").get(i);
		String count = ele.getText();
		ele.click();
		int result = Integer.parseInt(count);	
		//printv(result);
		//Verify the Table count Rows
		List<WebElement> allpacklist = locateElements("xpath","//table[@id='adminTable']//tbody/tr");
		int size = allpacklist.size();
		if(size==result)
		{
			print("Count is matched: "+size +" = "+result);
		}
	}
	locateElement("xpath","//tr[4]//img[@src='images/ViewPackage.png']").click();
	String status = locateElement("xpath","//p/span").getText();
	//print(status);
	if(status.equals("COMPLETED"))
	{
		print("This package has been completed");
	}
	WebElement element = locateElement("xpath","//div[@class='Logo']/a/img");
	action(element);
	WebElement Elemlog = locateElement("xpath","//input[@src='/images/Logout.png']");
	action(Elemlog);
	close();
}
}
