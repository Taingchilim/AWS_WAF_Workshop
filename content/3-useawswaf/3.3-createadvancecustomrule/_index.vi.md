+++
title = "បង្កើត Custom Rule កម្រិតខ្ពស់"
date = 2020
weight = 3
chapter = false
pre = "<b>3.3. </b>"
+++


#### ស្ថានភាព
ក្រុម Milkshake កំពុងបន្តវាយប្រហារកម្មវិធីរបស់អ្នក។ ពួកគេបានផ្លាស់ប្តូរវិធីវាយប្រហារម្តងទៀត! អ្នកត្រូវធ្វើបច្ចុប្បន្នភាព rule ដើម្បីទប់ស្កាត់សំណើអាក្រក់ទាំងនេះ ខណៈពេលដែលនៅតែអនុញ្ញាតឱ្យអតិថិជនពិតប្រាកដផ្ញើសំណើ។

#### បង្កើត Custom Rule កម្រិតខ្ពស់
{{% notice info %}} 
WAF Rule ទាំងអស់ត្រូវបានកំណត់ជា JSON Object។ សម្រាប់ rule ស្មុគស្មាញ វាងាយស្រួលជាងក្នុងការដោះស្រាយប្រសិនបើអ្នកធ្វើការដោយផ្ទាល់ជាមួយទម្រង់ JSON ជាជាងប្រើ Rule Editor នៅលើ console។ អ្នកអាចទាញយកព័ត៌មានរបស់ rule បច្ចុប្បន្នដែលត្រូវបានកំណត់ជា JSON តាមរយៈការប្រើ API, CLI ឬ Console ដោយប្រើពាក្យបញ្ជា [get-rule-group](https://docs.aws.amazon.com/cli/latest/reference/wafv2/get-rule-group.html)។ កែសម្រួលវាដោយប្រើកម្មវិធីកែសម្រួល JSON ដែលអ្នកចង់បាន និងផ្ទុកវាឡើងដោយប្រើពាក្យបញ្ជា [update-rule-group](https://docs.aws.amazon.com/cli/latest/reference/wafv2/update-rule-group.html) ដោយប្រើ API, CLI ឬ Console។\
ការកំណត់ rule ដោយប្រើ JSON អនុញ្ញាតឱ្យអ្នកអនុវត្តការគ្រប់គ្រងកំណែដើម្បីងាយស្រួលពិនិត្យមើលរបៀប ពេលវេលា និងមូលហេតុដែល rule ស្មុគស្មាញត្រូវបានផ្លាស់ប្តូរ។
{{% /notice %}}

លើកនេះ អ្នកនឹងចាប់ផ្តើមជាមួយ rule មូលដ្ឋានដូចខាងក្រោម៖\
Rule នេះនឹងទប់ស្កាត់សំណើទាំងអស់ដែលបំពេញលក្ខខណ្ឌទាំងពីរ៖
* មាន header ```x-milkshake: chocolate```
* មាន query parameter ```milkshake=banana```

1. នៅទំព័រព័ត៌មាន **Web ACL** របស់អ្នក។
* ចុច **Rules**។
* ចុច **Add Rules**។
* ចុច **Add my own rules and rule groups**។
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-001.png?featherlight=false&width=90pc)

2. នៅក្នុងផ្នែក **Rule builder**។
* ចុច **Rule JSON editor**។
* នៅក្នុងផ្នែក **JSON** បំពេញ 
```
{
  "Name": "complex-rule-challenge",
  "Priority": 0,
  "Action": {
    "Block": {}
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "complex-rule-challenge"
  },
  "Statement": {
    "OrStatement": {
      "Statements": [
        {
          "ByteMatchStatement": {
            "FieldToMatch": {
              "SingleHeader": {
                "Name": "x-milkshake"
              }
            },
            "PositionalConstraint": "EXACTLY",
            "SearchString": "chocolate",
            "TextTransformations": [
              {
                "Type": "NONE",
                "Priority": 0
              }
            ]
          }
        },
        {
          "ByteMatchStatement": {
            "FieldToMatch": {
              "SingleQueryArgument": {
                "Name": "milkshake"
              }
            },
            "PositionalConstraint": "EXACTLY",
            "SearchString": "banana",
            "TextTransformations": [
              {
                "Type": "NONE",
                "Priority": 0
              }
            ]
          }
        }
      ]
    }
  }
}
```
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-002.png?featherlight=false&width=90pc)

3. អូសអេក្រង់ចុះក្រោម ចុច **Add rule**។
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-003.png?featherlight=false&width=90pc)

4. ចុច **Save**។
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-004.png?featherlight=false&width=90pc)

#### ធ្វើបច្ចុប្បន្នភាព Rule
Rule នេះនឹងទប់ស្កាត់សំណើទាំងអស់ដែលបំពេញលក្ខខណ្ឌទាំងពីរ៖
* មាន header ```x-milkshake: chocolate``` និង header ```x-favourite-topping: nuts```
* មាន query parameter ```milkshake=banana``` និង query parameter ```favourite-topping=sauce```

1. នៅទំព័រព័ត៌មាន **Web ACL** របស់អ្នក។
* ចុច **Rules**។
* ចុច **complex-rule-challenge** (ឈ្មោះ rule ដែលត្រូវធ្វើបច្ចុប្បន្នភាព)។
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-005.png?featherlight=false&width=90pc)

2. ចុច **Edit**។
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-006.png?featherlight=false&width=90pc)

3. នៅក្នុងផ្នែក **Rule builder**។
* ចុច **Rule JSON editor**។
* នៅក្នុងផ្នែក **JSON** បំពេញ 
```
{
  "Name": "complex-rule-challenge",
  "Priority": 0,
  "Action": {
    "Block": {}
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "complex-rule-challenge"
  },
  "Statement": {
    "OrStatement": {
      "Statements": [
        {
          "AndStatement": {
            "Statements": [
              {
                "ByteMatchStatement": {
                  "FieldToMatch": {
                    "SingleHeader": {
                      "Name": "x-milkshake"
                    }
                  },
                  "PositionalConstraint": "EXACTLY",
                  "SearchString": "chocolate",
                  "TextTransformations": [
                    {
                      "Type": "NONE",
                      "Priority": 0
                    }
                  ]
                }
              },
              {
                "ByteMatchStatement": {
                  "FieldToMatch": {
                    "SingleHeader": {
                      "Name": "x-favourite-topping"
                    }
                  },
                  "PositionalConstraint": "EXACTLY",
                  "SearchString": "nuts",
                  "TextTransformations": [
                    {
                      "Type": "NONE",
                      "Priority": 0
                    }
                  ]
                }
              }
            ]
          }
        },
        {
          "AndStatement": {
            "Statements": [
              {
                "ByteMatchStatement": {
                  "FieldToMatch": {
                    "SingleQueryArgument": {
                      "Name": "milkshake"
                    }
                  },
                  "PositionalConstraint": "EXACTLY",
                  "SearchString": "banana",
                  "TextTransformations": [
                    {
                      "Type": "NONE",
                      "Priority": 0
                    }
                  ]
                }
              },
              {
                "ByteMatchStatement": {
                  "FieldToMatch": {
                    "SingleQueryArgument": {
                      "Name": "favourite-topping"
                    }
                  },
                  "PositionalConstraint": "EXACTLY",
                  "SearchString": "sauce",
                  "TextTransformations": [
                    {
                      "Type": "NONE",
                      "Priority": 0
                    }
                  ]
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-007.png?featherlight=false&width=90pc)

4. ចុច **Save rule**។
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-008.png?featherlight=false&width=90pc)

5. ចុច **Save**។
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-009.png?featherlight=false&width=90pc)

6. រត់ពាក្យបញ្ជា
```
# សំណើនេះនឹងត្រូវបានអនុញ្ញាត
curl -H "x-milkshake: chocolate" "<Your Juice Shop URL>"
curl  "<Your Juice Shop URL>?milkshake=banana"
```
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-010.png?width=60pc)
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-011.png?width=60pc)

7. រត់ពាក្យបញ្ជា
```
# សំណើនេះនឹងត្រូវបានទប់ស្កាត់
curl -H "x-milkshake: chocolate" -H "x-favourite-topping: nuts" "<Your Juice Shop URL>"
curl  "<Your Juice Shop URL>?milkshake=banana&favourite-topping=sauce"
```
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-012.png?width=60pc)
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-013.png?width=60pc)

សំណើដែលត្រូវបានទប់ស្កាត់នឹងមានការឆ្លើយតបដូចខាងក្រោម៖
```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
    <title>ERROR: The request could not be satisfied</title>
  </head>
  <body>
    <h1>403 ERROR</h1>
    <!-- Omitted -->
  </body>
</html>
```

នៅក្នុងផ្នែកនេះ អ្នកបានយល់ដឹងពីរបៀបកំណត់ WAF rule នៅក្នុងទម្រង់ JSON។ តក្កវិជ្ជាស្មុគស្មាញត្រូវតែកំណត់ដោយប្រើប្រតិបត្តិការ AND, OR និង NOT។