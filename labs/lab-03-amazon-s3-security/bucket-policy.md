{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowDeveloperReadWrite",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::760115946951:user/developer1"
            },
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::project-aegis-lab3-df-4821/*"
        }
    ]
}
