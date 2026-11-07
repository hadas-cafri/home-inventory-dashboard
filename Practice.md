# Practice – הנחיות עבודה עם Git ו-GitHub

ההנחיות האלה מגדירות איך שומרים, מעדכנים ומעלים את הפרויקט ל-GitHub.
הן מיועדות גם לי וגם ל-Claude Code, שעובד לפיהן כשהוא מבצע שינויים בפרויקט.

## 1. פרטי הריפו
- **שם:** `home-inventory-dashboard`
- **נראות:** פרטי (Private)
- **ענף ראשי:** `main` – תמיד מכיל גרסה עובדת.

## 2. הקמה ראשונית (פעם אחת)
```bash
cd home-inventory-dashboard
git init
git add .
git commit -m "Initial commit: spec, practice guide and dashboard"
gh repo create home-inventory-dashboard --private --source=. --remote=origin --push
```

## 3. כללי עבודה
1. **לא עובדים ישירות על `main`.** כל שינוי נעשה בענף נפרד.
2. **ענף לכל משימה**, עם שם שמתאר אותה:
   - `feature/<תיאור>` – פיצ'ר חדש (למשל `feature/add-item-form`)
   - `fix/<תיאור>` – תיקון באג (למשל `fix/expiry-badge`)
   - `docs/<תיאור>` – עדכון מסמכים
3. **commit קטן וממוקד** – כל commit עושה דבר אחד.
4. **בודקים לפני commit:** פותחים את `index.html` בדפדפן ומוודאים שהכול עובד.
5. **לא מעלים** סיסמאות, מפתחות API או מידע אישי אמיתי. הפרויקט עובד רק עם נתוני דמה.

## 4. הודעות commit
- באנגלית, בזמן הווה, עד כ-60 תווים.
- מתחילות בסוג השינוי:

| קידומת | מתי | דוגמה |
| --- | --- | --- |
| `feat:` | פיצ'ר חדש | `feat: add shopping list grouped by store` |
| `fix:` | תיקון באג | `fix: correct low-stock threshold` |
| `docs:` | מסמכים | `docs: update SPEC with order statuses` |
| `style:` | עיצוב בלבד | `style: improve mobile table layout` |
| `refactor:` | שינוי קוד בלי שינוי התנהגות | `refactor: split render functions` |

## 5. תהליך עבודה על שינוי
```bash
# 1. לעדכן את main
git switch main
git pull

# 2. לפתוח ענף חדש
git switch -c feature/add-item-form

# 3. לעבוד, לבדוק ולשמור
git add .
git commit -m "feat: add form for new inventory items"

# 4. להעלות ל-GitHub
git push -u origin feature/add-item-form

# 5. לפתוח Pull Request ולמזג
gh pr create --fill
gh pr merge --squash --delete-branch

# 6. לחזור ל-main המעודכן
git switch main
git pull
```

## 6. עדכון מסמכים
- כל שינוי בהתנהגות הדאשבורד מתעדכן גם ב-`SPEC.md`, באותו ענף.
- שינוי בתהליך העבודה מתעדכן ב-`Practice.md`.

## 7. הנחיות ל-Claude Code
- לקרוא את `SPEC.md` ואת `Practice.md` לפני כל משימה.
- לפתוח ענף חדש לכל משימה, ולא לבצע commit ישירות ל-`main`.
- להציג לי את השינויים ולקבל אישור לפני `push` ולפני מיזוג.
- לא למחוק ענפים או קבצים ולא להריץ `git push --force` בלי אישור מפורש.

## 7.1 בדיקה לפני העלאה לגיטהאב
בסיום כל עבודה, לפני `push`, יש לבדוק את הדשבורד בשני מצבים:

### בדיקה במצב דסקטופ (אתר)
1. פתח את `index.html` בדפדפן.
2. ודא שכל ה-KPI מוצגים נכון.
3. הוסף מוצר חדש עם כל השדות — ודא שמופיע בטבלה ומסטטוס נכון.
4. נסה להוסיף מוצר עם סף נמוך ≥ כמות תקין — ודא שמופיעה הודעת שגיאה.
5. עדכן תאריך תפוגה של מוצר קיים — ודא שמשתקף מיד.
6. בדוק שחיפוש וסינון עובדים.
7. מחק מוצר — ודא שמבקש אישור ונמחק.

### בדיקה במצב סלולרי (מובייל)
1. ב-Chrome/Firefox: פתח DevTools → לחץ על אייקון המובייל (Toggle device toolbar).
2. בחר רוחב 375px (iPhone) או 390px.
3. ודא שמוצגים **כרטיסים** ולא טבלה.
4. ודא שניתן להוסיף מוצר מהמובייל.
5. ודא שניתן לעדכן תאריך תפוגה מהכרטיס.
6. ודא שאין גלילה אופקית בעמוד.

### העלאה לגיטהאב — רק לאחר שכל הבדיקות עברו
```bash
git add index.html SPEC.md Practice.md
git commit -m "feat/fix/docs: תיאור קצר"
git push -u origin <branch-name>
```
> אל תעלה לגיטהאב לפני שבדקת גם בדסקטופ וגם בסלולרי.

## 8. פתרון בעיות נפוצות
| בעיה | פתרון |
| --- | --- |
| `git push` מבקש סיסמה | `gh auth setup-git` |
| שכחתי לפתוח ענף ועשיתי commit ל-main (לפני push) | `git switch -c feature/<שם>` ואז `git switch main` ו-`git reset --hard origin/main` |
| קונפליקט במיזוג | לפתוח את הקובץ, לבחור את הגרסה הנכונה, למחוק את הסימונים `<<<` `===` `>>>`, ואז `git add .` ו-`git commit` |
| לראות מה השתנה | `git status` ו-`git diff` |
| לראות היסטוריה | `git log --oneline --graph` |

## 9. שיתוף ריפו פרטי
כדי שהמרצה יוכל לראות את הריפו: ב-GitHub נכנסים ל-**Settings → Collaborators → Add people** ומוסיפים את שם המשתמש שלו.
