[Back to root](../README.md)

# Short Writeup of Npgsql Binary COPY

When performing bulk imports such as importing 90,000 rows from the excel sheet (4Mb), it takes around 40 seconds using EF Core's AddRange.

Instead by using Npgsql's BinaryImport, it was reduced to 1 second.

Similarly, reading 90,000 rows with EF Core usually takes around 30 seconds.

Instead by using Npgsql's BinaryExport, it was reduced to 0.5 seconds.

## Repository Bulk Write Function for DataListRow

```csharp
public async Task BulkInsertAsync(IEnumerable<DataListRow> entities)
{
    var connection = (NpgsqlConnection)_context.Database.GetDbConnection();
    
    // Ensure the connection is open
    if (connection.State != ConnectionState.Open)
    {
        await connection.OpenAsync();
    }

    using (var writer = connection.BeginBinaryImport($"COPY \"DataListRows\" (\"ListSerialId\", \"DataListRowId\", " +
                                                     $"\"Key\", \"Desc\", \"Mpn\", \"Vpn\", \"Ean\", \"Id\", \"DateCreated\", \"DateUpdated\", \"DateDeleted\") FROM STDIN (FORMAT BINARY)"))
    {
        foreach (var entity in entities)
        {
            // Start writing a row
            writer.StartRow();
            
            // Add each column value for the entity
            writer.Write(entity.ListSerialId, NpgsqlTypes.NpgsqlDbType.Integer);
            writer.Write(entity.DataListRowId, NpgsqlTypes.NpgsqlDbType.Integer);
            writer.Write(entity.Key, NpgsqlTypes.NpgsqlDbType.Text);
            writer.Write(entity.Desc, NpgsqlTypes.NpgsqlDbType.Text);
            writer.Write(entity.Mpn, NpgsqlTypes.NpgsqlDbType.Text);
            writer.Write(entity.Vpn, NpgsqlTypes.NpgsqlDbType.Text);
            writer.Write(entity.Ean, NpgsqlTypes.NpgsqlDbType.Text);
            writer.Write(entity.Id, NpgsqlTypes.NpgsqlDbType.Uuid);
            writer.Write(entity.DateCreated, NpgsqlTypes.NpgsqlDbType.TimestampTz);
            writer.WriteNull();
            writer.WriteNull();
        }

        await writer.CompleteAsync();
    }
}
```