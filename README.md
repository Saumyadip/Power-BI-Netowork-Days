# Power-BI-NetworkDays - This version takes care of Weekend holidays

(StartDate as date, EndDate as date, Holidays as list) as number =>

let
	DateList = List.Dates(StartDate, Number.From(EndDate - StartDate), #duration(1,0,0,0)),
	RemoveWeekends = List.Select(DateList, each Date.DayOfWeek(_, Day.Monday) < 5),
	RemoveHolidays = List.RemoveItems(RemoveWeekends, Holidays),
	CountDays = List.Count(RemoveHolidays),

	CountDay = if List.Contains(Holidays,EndDate) = true then 
	CountDays - 1
	else if Date.DayOfWeek(EndDate, Day.Monday) >= 5 then 
	CountDays - 1
	else 
	CountDays,

	FinalCountDay = if CountDay <= 0 then 
	0
	else 
	CountDay
in
	FinalCountDay
