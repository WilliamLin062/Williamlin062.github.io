---
title: C#使用Post
date: 2023-05-23 14:23:58
tags: C#
---

# POST API

        public TResponseModel HttpPost<TRequestModel, TResponseModel>(string url, TRequestModel body)
        {
            string postBody = JsonConvert.SerializeObject(body);

            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(url);

            request.Method = "POST";

            request.ContentType = "application/json";

            using (var streamWriter = new StreamWriter(request.GetRequestStream()))

            {

                streamWriter.Write(postBody);

            }

            //發出Request

            var httpResponse = (HttpWebResponse)request.GetResponse();

            var streamReader = new StreamReader(httpResponse.GetResponseStream());

            var result = streamReader.ReadToEnd();

            TResponseModel res = JsonConvert.DeserializeObject<TResponseModel>(result);

            return res;

        }
