import "math/rand"

rand.Seed(time.Now().UnixNano())
randomNumber := rand.Intn(10)