# Cao Thanh Tung _DEK15_project_01

1. Sắp xếp các bộ phim theo ngày phát hành giảm dần rồi lưu ra một file mới
    
    ```bash
    csvsql --query "SELECT * FROM 'tmdb-movies' order by release_date desc" tmdb-movies.csv > sorted_release_date_query.csv
    ```
    
2. Lọc ra các bộ phim có đánh giá trung bình trên 7.5 rồi lưu ra một file mới
    
    ```bash
    csvsql --query "SELECT * FROM 'tmdb-movies' where vote_average > 7.5" tmdb-movies.csv > vote_greater_75.csv
    ```
    
3. Tìm ra phim nào có doanh thu cao nhất và doanh thu thấp nhất
    
    ```bash
    # highest revenue
    echo "Highest Revenue" && csvsql --query "SELECT original_title FROM 'tmdb-movies' order by revenue desc limit 1" tmdb-movies.csv | tail -1
    
    # lowest revenue
    echo "Lowest Revenue" && csvsql --query "SELECT original_title FROM 'tmdb-movies' order by revenue limit 1" tmdb-movies.csv | tail -1
    ```
    
4. Tính tổng doanh thu tất cả các bộ phim
    
    ```bash
    echo "Sum Revenue" && csvsql --query "SELECT sum(revenue) FROM 'tmdb-movies'" tmdb-movies.csv | tail -1
    ```
    
5. Top 10 bộ phim đem về lợi nhuận cao nhất
    
    ```bash
    echo "Top 10 Revenue" && csvsql --query "SELECT original_title FROM 'tmdb-movies' order by revenue desc limit 10" tmdb-movies.csv | tail -n +2
    ```
    
6. Đạo diễn nào có nhiều bộ phim nhất và diễn viên nào đóng nhiều phim nhất
    
    ```bash
    # Top 1 director
    echo "Top 1 director" && csvcut -c director tmdb-movies.csv | tail -n +2 | tr '|' '\n' | sed '/^"$/d' | sed '/^$/d' | grep -v '^""$' | sort | uniq -c | sort -nr | head -n 1 | awk '{for(i=2; i<=NF; i++) printf "%s ", $i; print ""}'
    
    #Result
    Top 1 director
    Woody Allen
    
    # Top 1 caster
    echo "Top 1 caster" && csvcut -c cast tmdb-movies.csv | tail -n +2 | tr '|' '\n' | sed '/^"$/d' | sed '/^$/d' | grep -v '^""$' | sort | uniq -c | sort -nr | head -n 1 | awk '{for(i=2; i<=NF; i++) printf "%s ", $i; print ""}'
    
    #Result
    Top 1 caster
    Robert De Niro
    ```
    
7. Thống kê số lượng phim theo các thể loại. Ví dụ có bao nhiêu phim thuộc thể loại Action, bao nhiêu thuộc thể loại Family, ….
    
    ```bash
    echo "Count Genre" && csvcut -c genres tmdb-movies.csv | tail -n +2 | tr '|' '\n' | sed '/^"$/d' | sed '/^$/d' | grep -v '^""$' | sort | uniq -c | sort -nr
    
    # Result
    Count Genre
    4761 Drama
    3793 Comedy
    2908 Thriller
    2385 Action
    1712 Romance
    1637 Horror
    1471 Adventure
    1355 Crime
    1231 Family
    1230 Science Fiction
     916 Fantasy
     810 Mystery
     699 Animation
     520 Documentary
     408 Music
     334 History
     270 War
     188 Foreign
     167 TV Movie
     165 Western
    ```
    
8. Idea của bạn để có thêm những phân tích cho dữ liệu?
    - Top 5 film có doanh thu / vốn cao nhất
    - Top 10 thể loại film phổ biến trong từng khoảng thời gian 5 năm (2000-2005, 2005-2010, …)
    - Top 10 phim lợi nhuật trên từng phút cao nhất
    - Top 10 hãng sản xuất nhiều film nhất
