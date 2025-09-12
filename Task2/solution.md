# Задание 2

```mermaid
C4Context
System_Boundary(MEDIKAMENTE, "МЕДИКАМЕНТЕ") {
  
  System_Boundary(FileServerBoundary, "File Server") {
    System_Boundary(FileSystemBoundary, "File System") {

      System_Boundary(JournalsBoundary, "Journals") {
        System(JournalDoctorFIO, "Journal-Doctor-FIO", "Excel-журнал записи пациентов к специалисту")
      }

      System_Boundary(PatientBoundary, "Patient") {
        System(PatientsExcel, "Patients", "Excel-файл со списком пациентов")
        System_Boundary(PatientFilesBoundary, "Patient Files") {
          System(DataFile, "Data file", "Файлы пациента в различных форматах")
        }
      }

      System_Boundary(LaboratoryRegistryBoundary, "Laboratory Registry") {
        System(RegistryByDate, "RegistryByDate", "Реестр анализов за день")
      }
    }
  
    System(OneC_BukhgalteriyaPredpriyatiya, "1С Бухгалтерия предприятия", "Бухгалтерская система учёта. Работает в файловом режиме")
    System(OneC_TorgovlyaSklad, "1С Торговля и склад", "Складская система учёта. Работает в файловом режиме")
    System(ExchangeMailServer, "Exchange Mail Server", "Почтовый сервис внутренней почты")
  }

  System(KKM, "KKM", "Контрольно-кассовая машина")

  System_Boundary(PrivacySubsystem, "Privacy Management Subsystem") {
    System(PEG, "Privacy Enforcement Gateway", "Контролирует доступ к API и данным, реализует PbD")
    System(DPMS, "Data Privacy & Masking Service", "Обеспечивает обфускацию и маскирование PII/PHI")
    System(CAMS, "Consent & Audit Management System", "Управление согласием и аудитом доступа")
    System(IAM, "Identity and Access Management System", "Управление доступом, MFA, RBAC/ABAC")
  }

  System_Boundary(AnalyticsSubsystem, "Analytics Subsystem") {
    System(SDLT, "Secure Data Lake with Tagging", "Аналитический слой с безопасным хранением и обработкой данных по PbD")
    System(MAM, "Monitoring and Alerting Module", "Мониторинг безопасности, детекция инцидентов")
  }
}

Person(Administrator, "Administrator", "Сотрудник ресепшена")
Person(Bookkeeper, "Bookkeeper", "Бухгалтер")
Person(Cassier, "Cassier", "Кассир")
Person(Patient, "Patient", "Пациент")
Person(WarehouseKeeper, "WarehouseKeeper", "Сотрудник склада")
Person(Doctor, "Doctor", "Специалист-медик")
System_Ext(Laboratory, "Laboratory", "Медицинская лаборатория")

Rel(Administrator, JournalDoctorFIO, "Запись пациента к специалисту")
Rel(Administrator, PatientsExcel, "Регистрация пациента")
Rel(Administrator, DataFile, "Регистрация файла с данными пациента")
Rel(Doctor, JournalDoctorFIO, "Просмотр и редактирование журнала")
Rel(Patient, Cassier, "Оплата за медуслуги")
Rel(Cassier, KKM, "Прием оплаты")
Rel(Bookkeeper, OneC_BukhgalteriyaPredpriyatiya, "Проведение платежей")
Rel(WarehouseKeeper, OneC_TorgovlyaSklad, "Учет ТМЦ")
Rel(OneC_BukhgalteriyaPredpriyatiya, OneC_TorgovlyaSklad, "Внутриплатформенный обмен данных")
Rel(Patient, OneC_BukhgalteriyaPredpriyatiya, "Информация о приеме денежных средств")
Rel(OneC_TorgovlyaSklad, ExchangeMailServer, "Обмен уведомлениями")

Rel(Patient, PEG, "Взаимодействие через API клиентов и мобильное приложение")
Rel(PEG, CAMS, "Передача событий доступа и аудита")
Rel(PEG, IAM, "Проверка прав доступа и аутентификация")
Rel(PEG, DPMS, "Запросы с маскированием и защитой данных")
Rel(PEG, SDLT, "Запросы аналитики и исторических данных")
Rel(PEG, Laboratory, "Интеграция по API с контролем доступа")
Rel(SDLT, MAM, "Мониторинг и оповещение по безопасности данных")
Rel(CAMS, MAM, "Оповещения о подозрительных действиях")

Rel(IAM, Administrator, "Управление учётными записями и ролями")
Rel(IAM, Doctor, "Безопасный доступ с учётом ролей и политик")
Rel(IAM, Bookkeeper, "Безопасный доступ с учётом ролей и политик")
Rel(IAM, WarehouseKeeper, "Безопасный доступ с учётом ролей и политик")
Rel(IAM, Cassier, "Безопасный доступ с учётом ролей и политик")
Rel(IAM, Patient, "Самостоятельный доступ к своим данным через ЛК")

%% Стили для новых элементов и текста - красная обводка и красный цвет текста

UpdateElementStyle(PEG, $borderColor="red", $fontColor="red")
UpdateElementStyle(DPMS, $borderColor="red", $fontColor="red")
UpdateElementStyle(CAMS, $borderColor="red", $fontColor="red")
UpdateElementStyle(SDLT, $borderColor="red", $fontColor="red")
UpdateElementStyle(MAM, $borderColor="red", $fontColor="red")
UpdateElementStyle(IAM, $borderColor="red", $fontColor="red")

UpdateRelStyle(Patient, PEG, $textColor="red", $lineColor="red")
UpdateRelStyle(PEG, CAMS, $textColor="red", $lineColor="red")
UpdateRelStyle(PEG, IAM, $textColor="red", $lineColor="red")
UpdateRelStyle(PEG, DPMS, $textColor="red", $lineColor="red")
UpdateRelStyle(PEG, SDLT, $textColor="red", $lineColor="red")
UpdateRelStyle(PEG, Laboratory, $textColor="red", $lineColor="red")
UpdateRelStyle(SDLT, MAM, $textColor="red", $lineColor="red")
UpdateRelStyle(CAMS, MAM, $textColor="red", $lineColor="red")

UpdateRelStyle(IAM, Administrator, $textColor="red", $lineColor="red")
UpdateRelStyle(IAM, Doctor, $textColor="red", $lineColor="red")
UpdateRelStyle(IAM, Bookkeeper, $textColor="red", $lineColor="red")
UpdateRelStyle(IAM, WarehouseKeeper, $textColor="red", $lineColor="red")
UpdateRelStyle(IAM, Cassier, $textColor="red", $lineColor="red")
UpdateRelStyle(IAM, Patient, $textColor="red", $lineColor="red")
```


- Privacy Enforcement Gateway (PEG)
  
  Отдельный компонент, который маршрутизирует все запросы к API и данным, контролирует авторизацию и аутентификацию (RBAC, ABAC), фильтрует запросы по политикам доступа к конфиденциальной информации и ведет аудит. Защищает данные на уровне API интеграций (например, с лабораторией).
  
  Функции: внедрение Zero Trust, проверка прав, аудит, предотвращение утечек.

- Data Privacy and Masking Service (DPMS)
  
  Сервис для динамической маскировки и обфускации данных (например, PII, PHI) при отображении/обработке, в зависимости от ролей пользователя. Также поддерживает безопасное удаление данных по запросам (право на забвение) и контроль жизненного цикла данных.

- Consent and Audit Management System (CAMS)
  
  Управляет согласием пациентов на обработку данных, протоколирует события доступа и изменений с возможностью автоматического оповещения и анализа аномалий доступа.

- Secure Data Lake with Tagging (SDL-T)
  
  Озеро данных для BI/ML/AI с внедренным тегированием данных по категориям конфиденциальности, поддержкой мониторинга доступа, разграничением по ролям и атрибутам, формированием обезличенных/агрегированных выборок для аналитики.

- Monitoring and Alerting Module (MAM)
  
  Модуль централизованного мониторинга безопасности данных, детектирования инцидентов, генерации тревог и оповещений в режиме реального времени.

- Identity and Access Management System (IAM)
  
  Централизованное управление пользователями, ролями, привилегиями, безопасная аутентификация (MFA, OAuth2/OpenID Connect).
