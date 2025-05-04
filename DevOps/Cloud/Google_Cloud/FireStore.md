# Firestore

[![](https://lh5.googleusercontent.com/Yk_PQ60Rr82F--9tjwnxFNoO4ZFcMu1wsrwYkf2_14yvcy0lt_syXedMJ6OuUUx4EYFbe_Uz9yvzj2YBXQCpXnv4bBKhu0nNuGi-WcOYnrjBJzrz_AkfVcQZfDDAE03k-LluyiEg2INZoLZya4SqD_sE9pC7EbYs6oLZxmfjQrlUdB57K1nOovaJcX7KKw)](https://lh5.googleusercontent.com/Yk_PQ60Rr82F--9tjwnxFNoO4ZFcMu1wsrwYkf2_14yvcy0lt_syXedMJ6OuUUx4EYFbe_Uz9yvzj2YBXQCpXnv4bBKhu0nNuGi-WcOYnrjBJzrz_AkfVcQZfDDAE03k-LluyiEg2INZoLZya4SqD_sE9pC7EbYs6oLZxmfjQrlUdB57K1nOovaJcX7KKw)

[![](https://lh5.googleusercontent.com/E_ptp5YXWx9s5Ie3ANwmDxoM3veZYVk5ri9uI6czokJAg22ZZNVuPuK36Cp--bcNlzHAc-DbO5Pm0b8hj9QsKV2cpfTES33JZiwd3DRmFKUFbG0x-lEA6ZZMk8mblwjvQjnmmPUOCGj02xGHfRMHJmpT-hOd_v7tb3jm-wIINkhXR5TdihZI_5q1m1fm2g)](https://lh5.googleusercontent.com/E_ptp5YXWx9s5Ie3ANwmDxoM3veZYVk5ri9uI6czokJAg22ZZNVuPuK36Cp--bcNlzHAc-DbO5Pm0b8hj9QsKV2cpfTES33JZiwd3DRmFKUFbG0x-lEA6ZZMk8mblwjvQjnmmPUOCGj02xGHfRMHJmpT-hOd_v7tb3jm-wIINkhXR5TdihZI_5q1m1fm2g)

[![](https://lh4.googleusercontent.com/NSqhqLX4mvt6PmKL_On4U6ciQJTeBkzyr0oORgMY8RmZn7vO44kSyRFPedejLIS4Hf7uzN2KSDMZrR4vtYh0PkhUTlLBGPHgFpP_I43d_SRSRQ7m0NlO51iGxAzDKPjbQMTMF_AKqo95HCWJ3m1ysoG_8SGN6oTqqLZgv0HauftP6l_dgBoWtXpC9BFkaw)](https://lh4.googleusercontent.com/NSqhqLX4mvt6PmKL_On4U6ciQJTeBkzyr0oORgMY8RmZn7vO44kSyRFPedejLIS4Hf7uzN2KSDMZrR4vtYh0PkhUTlLBGPHgFpP_I43d_SRSRQ7m0NlO51iGxAzDKPjbQMTMF_AKqo95HCWJ3m1ysoG_8SGN6oTqqLZgv0HauftP6l_dgBoWtXpC9BFkaw)

gsutil versioning  set on  gs://qwiklabs-gcp-03-94190664b580

gsutil lifecycle get gs://qwiklabs-gcp-03-94190664b580

{"rule": [{"action": {"type": "Delete"}, "condition": {"age": 31}}]}

gsutil cp gs://qwiklabs-gcp-03-94190664b580/setup2.html recover2.html

gsutil cp -v setup.html gs://$BUCKET_NAME_1

gsutil ls -a gs://qwiklabs-gcp-03-94190664b580/setup.html

gs://qwiklabs-gcp-03-94190664b580/setup.html\#1665511608973949

gs://qwiklabs-gcp-03-94190664b580/setup.html\#1665514094478613

gs://qwiklabs-gcp-03-94190664b580/setup.html\#1665514178702289

export VERSION_NAME=gs://qwiklabs-gcp-03-94190664b580/setup.html\#1665511608973949

gsutil cp $VERSION_NAME recovered.txt

gsutil rsync -r ./firstlevel gs://qwiklabs-gcp-03-94190664b580/firstlevel

gcloud auth activate-service-account --key-file credentials.json

[![](https://lh6.googleusercontent.com/GUj79VI6TaQOBanwoI3slY3NHaecRkr6j7JvP2lR_eJuJ90oDNQ0Pb6uEBcvNx0jC1TwfTvbBbM6rmfAJwh_F0viuVNQYp5KOQhZTmkSQzoU7fJRpizDk1EMwXfMs57sM4-LtfL8VoUgWSSRMfRXYMydpSyA0YPNx9z2NqXfENBZcaronbNLpRoGBqPq)](https://lh6.googleusercontent.com/GUj79VI6TaQOBanwoI3slY3NHaecRkr6j7JvP2lR_eJuJ90oDNQ0Pb6uEBcvNx0jC1TwfTvbBbM6rmfAJwh_F0viuVNQYp5KOQhZTmkSQzoU7fJRpizDk1EMwXfMs57sM4-LtfL8VoUgWSSRMfRXYMydpSyA0YPNx9z2NqXfENBZcaronbNLpRoGBqPq)

[![](https://lh5.googleusercontent.com/cQ7Wki-XlSJADlpVn2rZGHTjEEedLMJWToHKLC27AKyqWKE-iiZr34rhgszPJsuSS9LtWDxg8A1-zf6b4KER8LkG-dp8WL9J56i6y_BqD0yttwZuf_TSkCEMYq4QrSj446oFYO8BdmGNQ7KW-RwLE6CzJpr1RQPmAJ7KhWwHYWyWXeQbiNZC3nuJ3qBPhQ)](https://lh5.googleusercontent.com/cQ7Wki-XlSJADlpVn2rZGHTjEEedLMJWToHKLC27AKyqWKE-iiZr34rhgszPJsuSS9LtWDxg8A1-zf6b4KER8LkG-dp8WL9J56i6y_BqD0yttwZuf_TSkCEMYq4QrSj446oFYO8BdmGNQ7KW-RwLE6CzJpr1RQPmAJ7KhWwHYWyWXeQbiNZC3nuJ3qBPhQ)

[![](https://lh5.googleusercontent.com/YE2V5ca5P8xzvqU7oF9cZPhNJ_Rth2pcN81xsDIYQb5KfdITuWbqqeTv1v7cKaxNufMqDe0hpTL2THawlyxnpftjMKyRuof3VORvfdnjC6CtHXHzbahh6SD_arD70usluHsEwRhhmvaLKU4iQv1glehmCJUrEp70oiMd57GVV8mUSG-U9GDAvvZzRcoR5g)](https://lh5.googleusercontent.com/YE2V5ca5P8xzvqU7oF9cZPhNJ_Rth2pcN81xsDIYQb5KfdITuWbqqeTv1v7cKaxNufMqDe0hpTL2THawlyxnpftjMKyRuof3VORvfdnjC6CtHXHzbahh6SD_arD70usluHsEwRhhmvaLKU4iQv1glehmCJUrEp70oiMd57GVV8mUSG-U9GDAvvZzRcoR5g)