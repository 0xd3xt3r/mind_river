# Qualcomm Interview

## Round 1
1. What is stack overflow? how does it work
2. Exploit mitigation technique (ASLR, ROP)
3. How to do ROP

## Round 2
Interviewer : Murali Somachy

Find Bugs in below code snippet

![[Screenshot from 2021-04-10 20-06-24.png]]

![[Screenshot from 2021-04-10 20-22-12.png]]

```
struct sock_addr {
	short sin_family;
	u_short sin_port;
}

int port = atol(argv[1]);
bool is_root;
if (port < 1024 && !is_root)
   return
   
sockaddr.sin_port = port;
```

## Round 3
Interviewer : Nilo Redine
1. Null dereference
2. what will you look bugs for in Linux based firmware?
3. How would you reverse bare-metal firmware?
4. What is stack overflow? how does it work
5. Exploit mitigation technique (ASLR, ROP)
6. How to do ROP

![[Screenshot from 2021-04-12 21-50-25.png]]

## Round 4
```C
enum CREDENTIAL { USERNAME, PASSWORD };

typedef struct { short type; size_t len; char buf[0]; } cred;

static const char ADMIN[] = "adminadmin";
int net_log_fd /* = TCP socket to the logging server */;

cred* make_cred(char *buf, int len, enum CREDENTIAL type)
{
	cred c;
    if (len > 200)
        return NULL;
    strcpy(c->buf, buf);
    c->type = type;
    c->len = len;
    return c;
}

void write_cred(int fd, cred *c)
{
    write(fd, c, 2 + 4 + c->len);
}

void read_cred_buf(int fd, cred *c)
{
    read(fd, c->buf, c->len);
}

int foo(char *user, char *pass, int user_len, int pass_len)  // entry
{
    cred *cu = make_cred(user, user_len, USERNAME);
    cred *cp = make_cred(pass, pass_len, PASSWORD);
    if (!cu || !cp)
        return 0;
    
    write(net_log_fd, "GOT_AUTH", 8);
    write_cred(net_log_fd, cu);
    write_cred(net_log_fd, cp);
    
    char concat[401];
    strcpy(concat, user);
    strcat(concat, pass);
    return (strncmp(concat, ADMIN, user_len + pass_len) == 0);
}

int main(int argc, char *argv[])
{
    int passed = foo(argv[1], argv[2], atoi(argv[3]), atoi(argv[4]));
    if (passed)
        printf("User AUTHENTICATED: ");
    else printf("Invalid username or password for: ");
    printf(argv[1]);
    printf("\n");
    return passed;
}
```


## Round 4
Interviewer : Yash shrivastav
1. Kernel code review. 

## Round 6

nsen@qti.qualcomm.com

1. Explain buffer overflow to your grandma
2. Explain camera vulnerability to your grandma
3. Asked me question about code coverage
4. how would you do protocol fuzzing
5. how would you find other type of bugs like memory leak,
6. how would you do static code analysis.
7. My training delivering experience why i did that.


## Round 7
1. how to detect bugs while fuzzing which doesn't generate crash
2. address SANITIZER, heap SANITIZER 


```

```