Data Cleaning (irra_UEZ6)

Download raw data

#merged two files
csvstack 2019-Oct-sample.csv 2019-Nov-sample.csv > data_merged.csv

#filter and reorder
csvcut -c 2,3,4,5,7,8,6 data_merged.csv > data_reorder.csv

#filter event_type column to purchase
csvgrep -c "event_type" -m "purchase" data_reorder.csv > data_purchase.csv

#cut category_code column
csvcut -c 7 data_purchase.csv > category_code.csv

#create category column
cat category_code.csv | awk -F "." '{print $1}' > category.csv

#create product_name column
cat category_code.csv | awk -F "." '{print $NF}' > product_name.csv

#join 3 csv files
csvjoin data_purchase.csv category.csv > joint1.csv
csvjoin joint1.csv product_name.csv > joint2.csv

#cut unneeded column
csvcut -c 1,2,3,4,5,6,8,9 joint2.csv > data_cleaned0.csv

#rename column
sed -e '1s/category_code2/category/' -e '1s/category_code2_2/product_name/' data_cleaned0.csv > data_cleaned.csv

#word count
cat data_cleaned.csv | wc > wc.txt

#validasi filter brand
cat data_cleaned.csv | grep electronics | grep smartphone | awk -F ',' '{print $5}'|sort|uniq -c | sort -nr > filter.txt
