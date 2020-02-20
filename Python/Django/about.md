## DB queryロギング
```
        import logging
        l = logging.getLogger('django.db.backends')
        l.setLevel(logging.DEBUG)
        l.addHandler(logging.StreamHandler())
```


## CSV export

```
import csv
import urllib
from datetime import datetime
from collections import OrderedDict
from django.http import StreamingHttpResponse


class Echo:
    def write(self, value):
        return value


class CsvGenerator(object):
    headers = OrderedDict([
        ('accommodation_id', '施設ID'),
        ('url', 'URL'),
    ])

    def __init__(self, accommodations):
        self.accommodations = accommodations
        self.region_id_area_name_map = RegionsService.get_region_id_area_name_map()
        self.accommodation_types = dict(AccommodationsService.get_accommodation_types())
        self._generator = self._get_generator()

    def __iter__(self):
        return self

    def __next__(self):
        return next(self._generator)

    def _process_chunk(self, chunk):
        # csv出力に必要な別テーブルの情報をまとめて取得
        accommodation_ids = [a['accommodation_id'] for a in chunk]
        accommodation_regions_map = AccommodationsService.get_accommodation_regions_map(accommodation_ids)

        country_region_ids = [a['country_region_id'] for a in chunk]
        country_id_area_name_map = RegionsService.get_country_id_area_name_map(country_region_ids)

        for accommodation in chunk:
            data = [getattr(accommodation['location'], key, '')
                    if key == 'x' or key == 'y'
                    else accommodation.get(key, '')
                    for key in self.headers.keys()]
            yield data

    def _get_generator(self):
        yield list(self.headers.values()) + list(self.ex_headers.values())
        chunk_size = 10000 # 1度に取得するaccommodationsのサイズ
        start = 0
        while True:
            chunk = self.accommodations[start:start + chunk_size]
            for row in self._process_chunk(chunk.values()):
                yield row
            if chunk.count() < chunk_size:
                raise StopIteration
            start += chunk_size


class DownloadCsv(AuthRequiredView):
    """
    作業者:データダウンロード@CSVダウンロード処理
    """
    method_only = ['GET']
    def main(self):
        params = self.request.GET.dict()
        form = SearchForm(params)
        form.is_valid()
        # 降順でソートするとDL中にデータが追加された場合ずれる可能性があるのでaccommodation_id昇順に並び替える
        self.items = Service.search(form.cleaned_data).order_by('accommodation_id').prefetch_related(None)

    def response(self):
        today = datetime.today()
        filename = urllib.parse.quote((
            f'csvfile_{today.year}{today.month}{today.day}.csv').encode("utf_8_sig"))
        pseudo_buffer = Echo()
        writer = csv.writer(pseudo_buffer)
        csv_generator = CsvGenerator(self.items)
        response = StreamingHttpResponse((writer.writerow(row) for row in csv_generator),
                                         content_type="text/csv; charset=utf_8_sig")
        response['Content-Disposition'] = 'attachment; filename*=UTF-8\'\'{}'.format(
            filename)
        return response
```
