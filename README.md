# Live Project

## Introduction
For the last two weeks of my time at the tech academy, I worked with as an Intern for Prosper IT consulting with my peers in a team developing a full scale ASP.Net MVC and code first Entity Framework Web Application in C#. It was a great learning oppertunity working on a legacy codebase  for adding requested features, cleaning up code, and fixing bugs. There were some complex stories that could have been a large time sink, but we worked trough them to deliver what was needed on time. I worked on several [back end stories](#back-end-stories) that challenged me and I am very proud of. A good portion of the site had already been built, but there were plenty of [front end stories](#front-end-stories) that required UI/UX improvements, all of varying degrees of difficulty. I saw how a quality project was made bygood developers working with what they have. Over the two week sprint I also had the opportunity to work on other [skills](#other-skills) like project management and team programming that I'm confident I will use in future projects.
  
Descriptions of the stories I worked on, along with code snippets and navigation links follow.

*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills), [Page Top](#live-project)*

## Back End Stories
* [BulkAdd Fixes and Features](#bulkadd-fixes-and-features)
* [Add favorite tracking system](#added-favorite-tracking-system)


### BulkAdd Fixes and Features
The BulKAdd page had a production selection dropwown that would populae part of the form but would not reset when selecting N/A and would display Invalid date if the production did not have a valid time for Matinee or Evening show. 
Javascript code to fix this:
```javascript
if (data != "[]") {
	let production = jQuery.parseJSON(data);
	let openingDay = production[0].OpeningDay.substr(0, 10); // This removes the time of day and leaves only the date
	let closingDay = production[0].ClosingDay.substr(0, 10);
	$("#generate__start-date-field").val(openingDay);
	$("#generate__end-date-field").val(closingDay);
	var mat = moment(production[0].ShowtimeMat).format('h:mm a');
	if (mat != "Invalid date") {
		$("label#matinee").show();
		$("input#matinee").show();
		$("#matinee-time").html(mat);
	} else {
		$("label#matinee").hide();
		$("input#matinee").hide();
		$("#matinee-time").html("");
	}
	var eve = moment(production[0].ShowtimeEve).format('h:mm a');
	if (eve != "Invalid date") {
		$("label#evening").show();
		$("input#evening").show();
		$("#evening-time").html(eve);
	} else {
		$("label#evening").hide();
		$("input#evening").hide();
		$("#evening-time").html("");
	}
	runtime = production[0].Runtime;
	$("#custom-time").val("");  /* added following block to clear area on change */
}
else {
	/* clears whole form when selecting n/a production */
	$("#generate__start-date-field").val("");
	$("#generate__end-date-field").val("");
	$('#matinee').prop("checked", false);
	$("label#matinee").show();
	$("input#matinee").show();
	$("#matinee-time").html("");
	$('#evening').prop("checked", false);
	$("label#evening").show();
	$("input#evening").show();
	$("#evening-time").html("");
	$("#custom-time").val("");
	$("#interval").val("1");
	$('#monday').prop("checked", false);
	$('#tuesday').prop("checked", false);
	$('#wednesday').prop("checked", false);
	$('#thursday').prop("checked", false);
	$('#friday').prop("checked", false);
	$('#saturday').prop("checked", false);
	$('#sunday').prop("checked", false);
}
```
Cstml code to fix display into a left justified form that is centered with responsiveness:
```HTML
<div class="bulk-form justify-content-start text-left pl-2">
@* Added bulk_form and replaced section *@
<h4 class="text-center">Generate Showtimes</h4>


<div class="row d-flex">
	<label class="col-sm-auto">Production</label>
</div>
<div class="row d-flex">
	<div class="col-sm-auto mx-0">
		@Html.DropDownList("Productions", (IEnumerable<SelectListItem>)ViewData["Productions"], "N/A", htmlAttributes: new { @class = "form-control", @id = "generate__production-field" })
		@Html.ValidationMessageFor(model => model.ProductionId, "", new { @class = "text-white" })
	</div>
</div>

<div class="row d-flex">
	<label class="col-sm-auto">Start Date</label>
</div>
<div class="row d-flex">
	<div class="col-sm-auto mx-0">
		@Html.EditorFor(model => model.StartDate, new { htmlAttributes = new { @class = "form-control", @id = "generate__start-date-field", @type = "date" } })
		@Html.ValidationMessageFor(model => model.StartDate, "", new { @class = "text-white" })
	</div>
</div>

<div class="row d-flex">
	<label class=" col-sm-auto">End Date</label>
</div>
<div class="row d-flex">
	<div class="col-sm-auto mx-0">
		@Html.EditorFor(model => model.EndDate, new { htmlAttributes = new { @class = "form-control ", @id = "generate__end-date-field", @type = "date" } })
		@Html.ValidationMessageFor(model => model.EndDate, "", new { @class = "text-white" })
	</div>
</div>

<div class="row d-inline-flex mx-0 show-times w-100 align-items-center">
	<label for="matinee" id="matinee" class="col-sm-auto mx-0">
		<input type="checkbox" id="matinee" name="matinee" />
		Matinee <span id="matinee-time"></span>
	</label>
	<label for="evening" id="evening" class="col-sm-auto mx-0">
		<input type="checkbox" id="evening" name="evening" />
		Evening <span id="evening-time"></span>
	</label>
	<label for="other" id="showtime-dropdown" class="col-sm-auto mx-0">Custom</label>
	<div class="col-sm-auto ml-2">
		@Html.DropDownList("custom-time", new SelectList(ViewData["Times"] as List<string>), "", new { @class = "form-control" })
	</div>
</div>

<div class="row d-flex">
	<div class="col-sm-auto">
		<label>
			@Html.CheckBox("monday", false)
			Monday
		</label>
	</div>
	<div class="col-sm-auto">
		<label>
			@Html.CheckBox("tuesday", false)
			Tuesday
		</label>
	</div>
	<div class="col-sm-auto">
		<label>
			@Html.CheckBox("wednesday", false)
			Wednesday
		</label>
	</div>
	<div class="col-sm-auto">
		<label>
			@Html.CheckBox("thursday", false)
			Thursday
		</label>
	</div>
	<div class="col-sm-auto">
		<label>
			@Html.CheckBox("friday", false)
			Friday
		</label>
	</div>
	<div class="col-sm-auto">
		<label>
			@Html.CheckBox("saturday", false)
			Saturday
		</label>
	</div>
	<div class="col-sm-auto">
		<label>
			@Html.CheckBox("sunday", false)
			Sunday
		</label>
	</div>
</div>

<div class="row d-flex ml-0 justify-content-start">
	<div class="col-sm-auto">
		<label>
			Interval
			<select id="interval">
				<option value="1">One Week</option>
				<option value="2">Two Weeks</option>
				<option value="3">Three Weeks</option>
				<option value="4">Four Weeks</option>
			</select>
		</label>
	</div>
</div>

<div class="row d-flex">
	<div class="col text-center mb-3">
		<button type="button" id="generate-button">Generate Showtimes</button>
		<br />
	</div>
</div>
```
CSS code:
```CSS
/* added bulk-form to contain the form area */
.bulk-form {
    display: block;
    margin-left: auto;
    margin-right: auto;
    margin-top: 20px;
    text-align: center;
    max-width: 50%;
    background-color: var(--main-secondary-color);
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
    border-radius: 2px;
    animation-duration: 1s;
    -webkit-animation-duration: 1s;
    animation-name: fadeIn;
    -webkit-animation-name: fadeIn;
}
```
 
 ### Added favorite tracking system
I was tasked with Adding favorite tracking system for logged in users.  I had to add a string field to the IdentityModel.  Add code to te cshtml to check for logged in user and display te heart overlay including the outline or solid heart and save the cast memver ID to the new field in the database.

C# code to add FavoriteCastMembers as a string to the database:
```CS
public string FavoriteCastMembers { get; set; } //String to store cast ids for favorites
```
C# code for ajax calls:
```CS
public Boolean ToggleFavorite(int? id)
{
	int castid = (int)id;
	var userManager = new UserManager<ApplicationUser>(new UserStore<ApplicationUser>(db));
	ApplicationUser currentUser = userManager.FindById(User.Identity.GetUserId());
	string favstr = currentUser.FavoriteCastMembers;
	List<int> favs = new List<int>();
	Boolean fav=false;
	if (favstr != null && favstr != "")
	{
		favs = favstr.Split(',').Select(int.Parse).ToList();
	}
		if (favs.Exists(element => element == id))
		{
			favs.Remove(castid);
			fav = false;
		}
		else
		{
			favs.Add(castid);
			fav = true;
		}


	favstr = String.Join(",", favs);
	currentUser.FavoriteCastMembers = favstr;
	db.SaveChanges();

	return fav;
}
//check if favorite and display correct heart. outline for non favorite or solid for favorite
public Boolean IsFav(int id)
{
	var userManager = new UserManager<ApplicationUser>(new UserStore<ApplicationUser>(db));
	ApplicationUser currentUser = userManager.FindById(User.Identity.GetUserId());
	string favstr = currentUser.FavoriteCastMembers;
	List<int> favs = new List<int>();
	Boolean fav;
	if (favstr != null && favstr != "")
	{
		favs = favstr.Split(',').Select(int.Parse).ToList();
	}
	fav = favs.Exists(element => element == id);
	return fav;
}
```

CShtml code to display overlay:
```HTML
<div class="cast-image-container">
	<img id="Isaiah_castImage" src="@Url.Action("DisplayPhoto", "Photo", new { id = Model.PhotoId })" class="cast-image" />
	@*Aded code to ceck if logged in and display favorite icon*@
	@if (User.Identity.IsAuthenticated) 
	{
		<div Class="cast-overlay">
			<a href="#" id="casticon" class="cast-icon" title="Favorite">
				 <i class="far fa-heart cast-heart" id="heart"></i>
			</a>
		</div>
	}
</div>
```
Javascript code to trigger ajax calls to read/save:
```javascript
$("#casticon").on("click", function () {
	var _this = $(".cast-icon");
	$.ajax({
		type: "POST",
		url: "/Admin/ToggleFavorite/" + @Model.CastMemberID,
		data:
		{
			id: @Model.CastMemberID,
		},
		success: function (data) {
			if (data == "True") {
				_this.find('i').removeClass('far');
				_this.find('i').addClass('fas');
			} else {
				_this.find('i').removeClass('fas');
				_this.find('i').addClass('far');
			}
		},
		error: function () {
			alert("An error has occurred.");
		},
	});
});
//display proper heart if favorite 
$(document).ready(function () {
	var _this = $(".cast-icon");
	$.ajax({
		type: "POST",
		url: "/Admin/IsFav/" + @Model.CastMemberID,
		data:
		{
			id: @Model.CastMemberID,
		},
		success: function (data) {
			if (data=="True")
			{
				_this.find('i').removeClass('far');
				_this.find('i').addClass('fas');
			} else {
				_this.find('i').removeClass('fas');
				_this.find('i').addClass('far');
			}
		},
		error: function () {
			alert("An error has occurred.");
		},
	});
	setInterval(showheart, 100);
});
// Delay do display only te correct heart (outline/solid)
function showheart() {
	document.getElementById("heart").style.visibility = 'visible'; 
}
```

*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills), [Page Top](#live-project)*


## Front End Stories
* [Fix Past Prodiction](#fix-past-production)
* [Modified Parts index](#modified-parts-index)

### Fix Past Production
This story was to fix past productions to be responsive with clamped text and ellipse.  Modified the cshtml to display up to 3 productions per row and clamp the description to 30 words wit ellipse if longer.

Cshtml code:
```HTML
<div class="card-body">
	<div class="ml-3 mr-3 justify-content-center pa-card-deck">
		@*Added custom deck*@
		<!-- Iterate through Productions and populate cards-->
		@foreach (var item in Model)
		{
			if (item.Season == i)
			{
				<div class="mb-4 align-items-center pa-card">
					@*Added custom card*@
					@*Added custon card*@
					@if (item.DefaultPhoto != null)
					{
						<div class="embed-responsive-4by3">
							<img src="@Url.Action("DisplayPhoto", "Photo", new { id = item.DefaultPhoto.PhotoId })" class="card-img-top arch-card-img bg-black" alt="@Url.Action(item.Title)">
						</div>
					}
					else
					{
						<div class="embed-responsive-4by3">
							<img src="~/Content/Images/Unavailable.png" class="card-img-top arch-card-img bg-black">
						</div>
					}
					<div class="card-body">
						@*<h5 class="card-title">@Html.DisplayFor(modelItem => item.Title)</h5>*@
						<h5 class="card-title">
							@item.Title
							<span class="badge badge-pill badge-info">Season: @item.Season</span>
						</h5>
						<p class="card-text" id="desc">
							@*Limit Description to 30 words*@
							@{
								var finalText = " ";
								var text = @item.Description;
								var text2 = text.Replace("  ", " ");
								var text3 = text2.Split(' ');
								var numberOfWords = text3.Length;
							}
							@if (numberOfWords > 30)
							{
								for (int k = 0; k < 30; k++)
								{
									finalText = finalText + text3[k] + " ";
								}
								@Html.Raw(finalText + '…');
							}
							else
							{
								@Html.Raw(text);
							}
						</p>
						<a href="@Url.Action("Details", "Productions", new { id = item.ProductionId })" class="stretched-link"></a>
					</div>
				</div>
			}
		}
	</div>
</div>
```

CSS code:
```CSS
/*card formatting for Arcive page*/
.pa-card {
    background-color: black;
    border-radius: 25px;
    height: 470px;
    min-width: 280px;
    max-width: 320px;
    -ms-flex: 1 0 0%;
    flex: 1 0 0%;
    margin-right: 15px;
    margin-bottom: 0;
    margin-left: 15px;
    position: relative;
    display: -ms-flexbox;
    display: flex;
    -ms-flex-direction: column;
    flex-direction: column;
    word-wrap: break-word;
}

/*custom card-deck formatting that allows custom format for screens < 575px*/
.pa-card-deck {
    display: -ms-flexbox;
    display: flex;
    -ms-flex-flow: row wrap;
    flex-flow: row wrap;
    margin-right: -15px;
    margin-left: -15px;
}
/*END CSS for Archive page*/
```

### Modified Parts index
This story was to modified the format of upper part of Parts index page and make it responsive and lined up. It as 3 dropdowns that needed to be centered and responsive and some text above and below that needed to be displayed closer to the dropdowns

Cshtml code:
```HTML
<div class="table-responsive">
    <table class="table-borderless mx-auto w-auto">
        <tbody>
            <tr>
                <td class="text-left">Filter By...</td>
            </tr>
            <tr>
                <td>
                    @*The HTML helper creates a form and when the form is submitted it calls the Index() method from PartController.*@
                    @*Had to apply a row to the form and move the col to a div o get the drop downs spaces out and adjusted the margis to pull top and bottm closer*@ 
                    @using (Html.BeginForm("Index", "Part", FormMethod.Post, new { @id = "partsForm", @class = "row mx-auto mt-n2 mb-n3" }))
                    {
                        // Creates a drop down list referencing the information from ViewData dictionary, which contains 'ProductionsID' key. The value that is selected is submitted onchange.
                        <div class="col-4 px-0">
                            @Html.DropDownList("ProductionsID", (IEnumerable<SelectListItem>)null, "All Productions", new { @class = "text-muted", @onchange = "document.getElementById('partsForm').submit();" })
                        </div>
                        <div class="col-4">
                            @Html.DropDownList("CastMembersID", (IEnumerable<SelectListItem>)null, "All Cast Members", new { @class = "text-muted", @onchange = "document.getElementById('partsForm').submit();" })
                        </div>
                        <div class="col-4 px-0">
                            @Html.DropDownList("Roles", (IEnumerable<SelectListItem>)null, "All Roles", new { @class = "text-muted", @onchange = "document.getElementById('partsForm').submit();" })
                        </div>
                            }
                    </td>
            </tr>
            <tr>
                <td class="text-right">@Html.ActionLink("Reset Filters", "ResetFilters")</td>
            </tr>
        </tbody>
    </table>
</div>
```

*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills), [Page Top](#live-project)*

## Other Skills
Skills relating to Team programming and project management
* Communicating about which files effect the story beig worked on.
* Working to the improve usability of an application with a group of developers to identify front and back end bugs
* Communicating with others team members or lead wen needed to clairfy someting or get anoter idea on the task/problem
* Learning new ways to improve effecency by examinging others work and asking questions  
  
*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills), [Page Top](#live-project)*
