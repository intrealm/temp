get the sheet
define current findingcodeDTO as empty;

for earch row from the sheet
  if(current is empty or row is empty ) //write protected method to check through columsn
    persist current if exists
    current  = create findingcodeDTO
    continue;
  set single row attributes here
  else
    add this rows's solutioncode to the current's solutionCode list   
