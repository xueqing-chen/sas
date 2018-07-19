%MACRO  freq_var(InputVar_List= , table=, result=);
%LET i =1;
%DO %WHILE (%SCAN(&InputVar_List, &i) NE );
	proc freq data=&table;
	table %SCAN(&InputVar_List, &i) /missing out=freq&i;

	data freq&i;
    set freq&i;
	length varname $100.;
	varname="%SCAN(&InputVar_List, &i)";
	rename %SCAN(&InputVar_List, &i)=value;
	run;
	proc append base=&result	data=freq&i force;
	run;
	proc delete data=freq&i;run;
%LET i = %EVAL(&i+ 1);		
%END;
		

%mend;
