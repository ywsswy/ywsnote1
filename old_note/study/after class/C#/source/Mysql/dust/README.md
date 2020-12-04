insert into patient_info (patient_name,patient_sex,patient_date,patient_bed,patient_nurse,patient_doctor)
values ('王五','男',now(),(select sickbed_room from sickbed_info where sickbed_status='空闲' limit 1 ),(select nurse_id from nurse_info where nurse_affair=(select min(nurse_affair) from nurse_info) limit 1),11);
设置默认值affair
