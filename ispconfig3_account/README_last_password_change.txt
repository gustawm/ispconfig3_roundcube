Needed database modifications:
Add a column to mail_user table:
ALTER TABLE mail_user ADD COLUMN `last_password_change` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP;

Create a trigger:
DELIMITER ;;
CREATE TRIGGER `tri_password_change_date` BEFORE UPDATE
    ON isp_db.mail_user FOR EACH ROW
    IF NOT (NEW.password <=> OLD.password) THEN
SET NEW.last_password_change = NOW();
     END IF;;
DELIMITER ;
