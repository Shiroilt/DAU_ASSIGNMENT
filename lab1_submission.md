# file to be saved as `202512040_lab1.md`

1. `todays_schedule.sh`
```bash
a=$(date +%A)
grep "$a" timetable.csv
```

2. `instructor_course_count.sh instructor_name`
```bash
echo "instructor name :"
read a
grep $a timetable.csv |cut -d',' -f3 |sort| uniq | wc -l

```
3. room_course_list.sh room_name
```bash
echo "Room num :"
read a
awk -F',' -v r="$a" '$5 == r { print $3 }' timetable.csv|uniq
```
4. `course_timetable.sh course_name`
```bash
echo "Course name :"
read a
grep $a timetable.csv 
```
## Intermediate
1. `room_empty_slots.sh room_name`
```bash
room="$1"
echo "Empty slots for room: $room"

d=$(grep $1 timetable.csv|cut -d',' -f1|sort|uniq)
s=$(grep $1 timetable.csv|cut -d',' -f2|sort|uniq)

for day in $d; do
  for slot in $s; do
    EXISTS=$(awk -F',' -v d="$day" -v t="$slot" -v r="$room" '$1==d && $2==t && $5==r' timetable.csv)
    
    if [ -z "$EXISTS" ]; then
      echo "$day,$slot is FREE"
    fi
  done
done

```
2. `room_class_count.sh`
```bash
echo "Room, Day, Count"
awk -F',' '{print $5 "," $1}' timetable.csv | sort | uniq -c | while read c l; do
    echo "$l,$c"
done

```
3. `instructor_location.sh instructor_name`
```bash
i="$1"

c=$(grep $1 timetable.csv|cut -d',' -f5|sort|uniq)
slots=('08:00-08:50' '09:00-09:50' '10:00-10:50' '11:00-11:50' '12:00-12:50')
d=$(grep $1 timetable.csv|cut -d',' -f1|sort|uniq)
for d in $d; do
  for slot in "${slots[@]}"; do
    EXISTS=$(awk -F',' -v d="$d" -v t="$slot" -v i="$1" '$1==d && $2==t && $4==i' timetable.csv)
    echo $d
    if [ -n "$EXISTS" ]; then
      echo "in class $slot"
    else
      echo "office   $slot"
    fi
    done
done
```

4. Find the student by id
id=$1

b=$(grep $id global_students.csv |cut -d',' -f2)

c=$(grep $b timetable_all_batches.csv|cut -d',' -f5|uniq)
echo $c
