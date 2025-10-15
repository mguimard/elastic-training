# Lab 3: Using ElasticSearch Pipelines and Kibana Charts

The goal of this lab is to learn how to create an ElasticSearch pipeline to populate an **Elasticsearch** index, execute advanced queries via the **DevTools console**, and create advanced visualizations in **Kibana**.

Files used (provided):

* Data file: `sncf-non-conformites.bulk`
* Index mapping: `sncf-mappings.txt`

## Creating the mappings

Analyze the file `sncf-mappings.txt` and execute it in the **DevTools console**.

Make any necessary modifications (field types, etc.).

## Creating the pipeline

Write an ElasticSearch pipeline named "sncf-pipeline" with the following processor :

- CSV (field message), name the fields, specify the csv separator

## Inserting data

Insert the data with a curl command, or from the DevTools.

```bash
curl -XPOST -u "elastic:CHANGEME" -k "https://localhost:9200/sncf/_bulk?pipeline=sncf-pipeline" -H"Content-Type: application/json" --data-binary @sncf-non-conformites.bulk
```

Or with DevTools:

```bash
POST sncf/_bulk?pipeline=sncf-pipeline
{"index": {}}
{"message": "2016-12-16T07:29:00+01:00;0087751081;Marseille Blancarde;63.0;1.0"}
{"index": {}}
{"message": "2016-12-15T10:44:00+01:00;0087765826;Entraigues-sur-la-Sorgue;26.0;0.0"}
{"index": {}}
{"message": "2016-12-15T11:07:00+01:00;0087756460;Roquebrune-Cap-Martin;51.0;5.0"}
{"index": {}}
{"message": "2016-12-15T16:49:00+01:00;0087384008;Paris Saint-Lazare;71.0;5.0"}
{"index": {}}
{"message": "2016-12-15T11:53:00+01:00;0087286005;Lille Flandres;66.0;2.0"}
# other lines ....
```

Check that the database has been correctly populated by exploring the data in **Kibana** or by using the following command:

```bash
GET sncf/_count
```

The next steps requires that you create a data view for this index.

## Kibana Charts

In **Kibana**, create new dashboard with the following chart :

- Display the **trend in the number of non-conformities**, month by month, across all stations.
- On the same chart: Display the **trend in the number of observations**.

## Adding virtual fields

Add these fields to the mapping:

```json
PUT sncf/_mapping
{
  "runtime": {
      "conformity_rate": {
        "type": "double",
        "script": {
          "source": """
          if( doc['observations'].size()!=0 && doc['issues'].size() != 0 && doc['observations'].value > 0 && doc['issues'].value > 0)
            emit(1.0* doc['issues'].value / doc['observations'].value);
          else
            emit(0);
          """
        }
      },
      "hour_of_day": {
        "type": "double",
        "script": {
          "source": """
          if(doc['@timestamp'].size()!=0 )
            emit(doc['@timestamp'].value.getHour());
          else
            emit(0);
          """
        }
      },
      "day_of_week": {
        "type": "keyword",
        "script": {
          "source": """
          if(doc['@timestamp'].size() == 0)
            emit('NA');
          else
          emit(doc['@timestamp'].value.dayOfWeekEnum.getDisplayName(TextStyle.FULL, Locale.ROOT));
          """
        }
      }
    }
}
```

Create a dashboard with the following charts:

* Evolution of the **conformity rate**
* Number of **issues by day of the week**
* Number of **issues by hour of the day**
* Metrics: count of unique stations, count of total observations, count of total issues.
