get the sheet
define current findingcodeDTO as empty;

for earch row from the sheet
  if findingcodeDTO is empty
    current = create findingcodeDTO
  if findingcodeDTO is not empty and row[column1] exists
    persist current
    current = create findungcodeDTO
    set single row attributes here
  else
    add this rows's solutioncode to the current's solutionCode list   
