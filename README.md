Background
As request, we should add feature to filter gross profit following month of year using GoodData.UI:
Goal
Collect GoodData data using GoodData API to show on GUI.
Plan
1.	Add a Month drop down list to GUI to support data collection
Required GUI Updates
•	Add Month drop down list select into GUI.
•	When select Drop down list, GUI must support OnChanged event to reload chart appropriately
•	GUI must show as below:
 
Apply in Source Code:
•	Adjust file App.js as below:
o	Add new Parameter:
state = {
fromDate: '2016-1-01',
toDate: '2016-1-31',

};
o	Adding function to execute drop down event:

onChangeGrossProfit(e) {
        let month = parseInt(this.menu.value);
        var fromDate = new Date(2016, month, 1).getDate();
        var toDate = new Date(2016, month, 0).getDate();
        var formatFromDate = "2016-" + month + "-" + fromDate; 
        var formatToDate = "2016-" + month + "-" + toDate; 
        //
        console.log("from: " + formatFromDate + " to: " + formatToDate);
        this.setState({
            fromDate: formatFromDate,
            toDate: formatToDate,
        });
    }
o	Add event in RenderDropdown function:

<select defaultValue="1" onChange={this.onChangeGrossProfit.bind(this)} ref={(input) => this.menu = input}>
                <option value="1">January</option>
                <option value="2">February</option>
                <option value="3">March</option>
                <option value="4">April</option>
                <option value="5">May</option>
                <option value="6">June</option>
                <option value="7">July</option>
                <option value="8">August</option>
                <option value="9">September</option>
                <option value="10">October</option>
                <option value="11">November</option>
                <option value="12">December</option>
            </select>

o	Edit getMonthFilter function: adding 02 parameters into function:

getMonthFilter(f, t) {
        return {
            absoluteDateFilter: {
                dataSet: {
                    uri: dateAttribute
                },
                from: f,
                to: t
            }
        }
    }

o	Edit Render function:

render() {
        const projectId = 'xms7ga4tf3g3nzucd8380o2bev8oeknp';
        const filters = [this.getMonthFilter(this.state.fromDate, this.state.toDate)];
        const measures = this.getMeasures();
        const viewBy = this.getViewBy();

        return (
            <div className="App">
                <h1>$ Gross Profit in month {this.renderDropdown()} 2016</h1>
                <div>
                    <ColumnChart
                        measures={measures}
                        filters={filters}
                        projectId={projectId}
                    />
                </div>
                <h1>$ Gross Profit - All months</h1>
                <div>
                    <ColumnChart
                        measures={measures}
                        viewBy={viewBy}
                        projectId={projectId}
                    />
                </div>
            </div>
        );
    }
Installing guide:
Requirements:
-	Have node.js and yarn installed in your development environment.
-	If you do not have these tools installed, you can get them from https://nodejs.org/  and https://yarnpkg.com/  respectively. You must install version 10.16 (stable version, cannot install version 12.04)
Installation Steps:
-	Download source from attached file of email
-	Point to folder gooddata-js-interview/goodata-js-interview_publish/goodata-js-interview/
-	Run yarn install
-	Run HTTPS=true yarn start (Linux, macOS) or set HTTPS=true&&npm start (Windows).
-	Visit https://localhost:3000/account.html , and log in using your GoodData account credentials.
o	If you do not have a GoodData account yet, create one.
o	After creating a GoodData account, Web can load data from GoodData.
-	Visit https://localhost:3000/
