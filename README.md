# sql_test
# 1. Создать таблицу пользователей
CREATE TABLE Users
(
    Id INT IDENTITY PRIMARY KEY NOT NULL,
	FirstName VARCHAR(30) NOT NULL,
    LastName VARCHAR(30) NOT NULL,
    Age INT NOT NULL,    
    Email VARCHAR(30),
    Phone VARCHAR(20) NOT NULL
);
INSERT INTO Users (FirstName, LastName, Age, Email, Phone) VALUES ('Fletch', 'Strelitzer', 17, 'fstrelitzer0@github.com', '714-654-4030');
INSERT INTO Users (FirstName, LastName, Age, Email, Phone) VALUES ('Isa', 'Doogood', 22, 'idoogood1@csmonitor.com', '427-410-6027');
INSERT INTO Users (FirstName, LastName, Age, Email, Phone) VALUES ('Izak', 'Biaggi', 23, 'ibiaggi2@cyberchimps.com', '719-453-7286');
INSERT INTO Users (FirstName, LastName, Age, Email, Phone) VALUES ('Margie', 'Salmond', 24, 'msalmond3@google.pl', '846-554-1739');
INSERT INTO Users (FirstName, LastName, Age, Email, Phone) VALUES ('Rickey', 'Duthie', 25, 'rduthie4@hatena.ne.jp', '452-519-9483');


#############################################################################################################################################
# 2. Создать таблицу счетов пользователей (у одного пользователя может быть несколько счетов)
CREATE TABLE Chet
(
	Id INT IDENTITY PRIMARY KEY NOT NULL,
    Nomer VARCHAR(50) NOT NULL,
	Balance BIGINT NOT NULL,
	UserId INT FOREIGN KEY REFERENCES Users(Id) NOT NULL
);
INSERT INTO Chet (Nomer, Balance, UserId) VALUES ('№111-1a', 17000, 1);
INSERT INTO Chet (Nomer, Balance, UserId) VALUES ('№222-2a', 10000, 2);
INSERT INTO Chet (Nomer, Balance, UserId) VALUES ('№222-2b', 10000, 2);
INSERT INTO Chet (Nomer, Balance, UserId) VALUES ('№111-1b', 11000, 1);
INSERT INTO Chet (Nomer, Balance, UserId) VALUES ('№111-1c', 17000, 1);
#######################################################################################
#3. Создать функцию по списанию денег с одного пользователя в пользу другого пользователя. (принимает сумму денег, счет отправителя, счет получателя)
CREATE FUNCTION Func
(
	@amount AS BIGINT,
	@chet_otprav AS VARCHAR(50),
	@chet_poluchatel AS VARCHAR(50)
)
RETURNS varchar(15)
AS
BEGIN
	UPDATE Chet SET Balance = Balance - @amount 
		WHERE Nomer = @chet_otprav  and Balance >= @amount;
	UPDATE Chet SET Balance = Balance + @amount
		WHERE Nomer = @chet_poluchatel
    RETURN  'Успешно' ;
END
