# Starting secondary test

> 19 dec 
> 
## Modify code to generalize testing operation

Update new code for plate and cylinder into `more_test` folder.

- Now the figures are exported to specific folders
- Rifined code

## To Do

- Changed load value (to adapt deformation)

> ⚠️ I have to make changes to apply differential load condition over the plate. 
> IDEA: use `FEMModel[]` to pass text variable like:
> - `AxialLoad->True/False`
> - `TransvLoad->True/False`

- Pass load from `FEMModel[]`
- Run simulation!