SELECT
  quarterStart,
  FacilityOHAIdentifier,
  PlaceOfServiceCode,
  CountyFipsCode,
  PayerGroupCode,
  MSDrgProductLineCode,
  MdcCode,
  MSDrgCode,
  SUM(TotalEncounters) AS TotalEncounters,
  COUNT(PatientID) AS TotalPatients
FROM (
  SELECT
    CONCAT(DateOfBirth,EthnicityCode,LanguageCode,RaceCode,SexCode,UsPostalCode) AS PatientID,
    FacilityOHAIdentifier,
    PlaceOfServiceCode,
    CountyFipsCode,
    PayerGroupCode,
    MSDrgProductLineCode,
    MdcCode,
    MSDrgCode,
    date_trunc(DischargeDate, QUARTER) as quarterStart,
    COUNT(RowId) AS TotalEncounters
  FROM
    `ohadata.Oha_data.IP`
  WHERE
    CountyFipsCode IN (39045, 39049, 39031, 39059, 39115, 39119, 39121, 39127, 39009, 39013, 39089, 39111, 39157, 39167, 39067, 39073, 39083)
  GROUP BY
    quarterStart,
    PatientID,
    FacilityOHAIdentifier,
    PlaceOfServiceCode,
    CountyFipsCode,
    PayerGroupCode,
    MSDrgProductLineCode,
    MdcCode,
    MSDrgCode)
GROUP BY
  quarterStart,
  FacilityOHAIdentifier,
  PlaceOfServiceCode,
  CountyFipsCode,
  PayerGroupCode,
  MSDrgProductLineCode,
  MdcCode,
  MSDrgCode
